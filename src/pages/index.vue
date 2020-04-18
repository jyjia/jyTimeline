<template>
  <div id="body">
    <div id="link">
      <a href="http://nfdsb.oeeee.com/h5/nandu/my/doctor/source/index.html">
        <img src="../../static/images/jnsz.png" class="jnszType">
      </a>
      <a href="http://nfdsb.oeeee.com/h5/nandu/my/doctor/source/index.html">
        <img src="../../static/images/zjhj.png" class="zjhjType">
      </a>
    </div>
    <div id="pageTitle">
      <div class="titleCenter">记疫</div>
    </div>
    <div id="navs" :class="{fixedBar: isFixed}">
      <my-navs @checkInfo="checkInfo"></my-navs>
    </div>

    <div id="timelineBody">
      <my-order @isReverse="reverseItems"></my-order>
      <div v-for="item in timelineInfo">
        <div class="yearType">{{item.year}} 年</div>
        <my-time-line :items="item.data">></my-time-line>
      </div>
      <div >
        <a href="javascript:;">
          <img id="moreInfo" src="../../static/images/moreinfo.png">
        </a>
      </div>
    </div>
    <div id="foot">
      @2020 by Jia Jingyi
      <br>
      疫情数据 时间轴
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import navs from '../components/navs'
import MyTimeLine from '../components/mytimeline'
import MyOrder from '../components/order'
export default  {
  components:{
    MyNavs: navs,
    MyTimeLine,
    MyOrder
  },
  data () {
    return {
      info: [],
      showInfo: [],
      items: [],
      timelineInfo: [],
      check: ['政府行动','境内疫情','行业战疫','境外疫情'],
      page: 1,
      status: 1,
      isFixed: false,
      isRev: false
    }
  },
  created(){
    this.page = 1;
    this.getInfo();
  },
  mounted(){
    this.$nextTick(function () {  //监听滚动事件
      window.addEventListener('scroll', this.onScroll)
    })
  },
  methods: {
    getInfo() {   //获取数据
      //拼接请求数据
      console.log(this.page);
      let infoUrl = "http://localhost:8080/static/epidemicInfo"+this.page+".json"
      axios
        .get(infoUrl)
        .then(response => {
          this.info.push(response.data.data);
          this.editInfo();
        })
        .catch(function (error) { // 请求失败处理
          console.log(error);
        });
    },
    editInfo(){  //处理数据
      let nowPage ;
      if(this.isRev){ //反序
        nowPage = 6-this.page;
        this.info[nowPage].reverse();
       /* for(let i=0;i<this.info[nowPage].length;i++){
          this.info[nowPage][i].reverse();
        }*/
      }else{   //正序
        nowPage = this.page-1;
      }
      for(let i=0;i<this.info[nowPage].length;i++){
        //不同类型，颜色显示不同
        if(this.info[nowPage][i].category === config[0].category ){
          this.info[nowPage][i].typeColor = config[0].typeColor;
        }else if(this.info[nowPage][i].category === config[1].category){
          this.info[nowPage][i].typeColor = config[1].typeColor;
        }else if(this.info[nowPage][i].category === config[2].category){
          this.info[nowPage][i].typeColor = config[2].typeColor;
        }else if(this.info[nowPage][i].category === config[3].category ){
          this.info[nowPage][i].typeColor = config[3].typeColor;
        }
        //修改时间格式
        this.info[nowPage][i].time_point = this.info[nowPage][i].time_point.split(' ')[0];

        //判断是否是重点数据
        if(this.info[nowPage][i].important === "0"){
          this.info[nowPage][i].isImport = false;
        }else{
          this.info[nowPage][i].isImport = true;
        }
        //判断是否是video
        if(this.info[nowPage][i].video){
          this.info[nowPage][i].isVideo = true;
        }else{
          this.info[nowPage][i].isVideo = false;
        }
      }

      this.showInfo = JSON.parse(JSON.stringify(this.info));
      this.editCheck();
    },
    checkInfo(data){   //根据选项筛选数据
      this.check = data;
      this.editCheck();
    },
    editCheck() {
      this.showInfo = JSON.parse(JSON.stringify(this.info));
      //console.log(this.showInfo);
      for(let m=0;m<this.showInfo.length;m++){
        let mark = 0;
        let len=this.showInfo[m].length;
        let i=0;
        while(i<len){
          for(let j in this.check) {
            if (this.showInfo[m][i].category === this.check[j]) {
              mark = 1;
            }
          }
          if(mark===0){ //此条数据不显示
            this.showInfo[m][i].isShow = true;
          }else {  //此条数据显示
            mark=0;
          }
          i++;
        }
      }
      this.reDupTime();
    },
    reDupTime() {   //处理数据，生成新格式的数据
      this.items = [];
      this.timelineInfo = [];
      let dateText=this.showInfo[0][0].time_point;  //时间
      let detailText = [];  //详细内容
      detailText[0]=this.showInfo[0][0];
      let year = this.showInfo[0][0].time_point.split('-')[0];
      let num=1;
      for(let m=0; m<this.showInfo.length;m++){
        for(let i=1;i<this.showInfo[m].length;i++){
          //根据日期处理数据，相同日期的放到一起
          if(this.showInfo[m][i].time_point === this.showInfo[m][i-1].time_point){
            detailText[num] = this.showInfo[m][i];
            num++;
          }else{
            this.items.push({
              date: dateText,
              time: dateText.split('-')[1]+"-"+dateText.split('-')[2],
              detail: detailText
            });
            dateText=this.showInfo[m][i].time_point;
            num=0;
            detailText = [];
            detailText[num]=this.showInfo[m][i];
          }
          if(i === this.showInfo[m].length-1){
            //最后的时间可能与下组数据的开始时间相同
            if(m < this.showInfo.length-1){
              if(dateText !== this.showInfo[m+1][0].time_point){
                this.items.push({
                  date: dateText,
                  time: dateText.split('-')[1]+"-"+dateText.split('-')[2],
                  detail: detailText
                });
                dateText=this.showInfo[m+1][0].time_point;
                num=0;
                detailText = [];
                detailText[num]=this.showInfo[m+1][0];
              }
            }else{
              this.items.push({
                date: dateText,
                time: dateText.split('-')[1]+"-"+dateText.split('-')[2],
                detail: detailText
              });
            }
          }

          //根据年份处理
          let tempYear = this.showInfo[m][i].time_point.split('-')[0];
          if(year !== tempYear){
              this.timelineInfo.push({
                year: year,
                data: this.items
              });
            year = this.showInfo[m][i].time_point.split('-')[0];
            this.items = [];
          }
        }

        if(m === this.showInfo.length-1) {
          this.timelineInfo.push({
            year: year,
            data: this.items
          })
        }
      }
      this.status = 1;
    },
    //反转
    reverseItems(data) {
      /*for(let i=0;i<this.info.length;i++){
        this.info[i].reverse();
      }*/
      if(data){ //反序展示
        this.page = 6;
        this.isRev = true;
        this.info = [];
        this.getInfo();
      }else{  //正序展示
        this.page = 1;
        this.isRev = false;
        this.info = [];
        this.getInfo();
      }
    },
    // 获取滚动条当前的位置
    getScrollTop () {
      let scrollTop = 0
      if (document.documentElement && document.documentElement.scrollTop) {
        scrollTop = document.documentElement.scrollTop
      } else if (document.body) {
        scrollTop = document.body.scrollTop
      }
      return scrollTop
    },
    // 获取当前可视范围的高度
    getClientHeight () {
      let clientHeight = 0
      if (document.body.clientHeight && document.documentElement.clientHeight) {
        clientHeight = Math.min(document.body.clientHeight, document.documentElement.clientHeight)
      } else {
        clientHeight = Math.max(document.body.clientHeight, document.documentElement.clientHeight)
      }
      return clientHeight
    },

    // 获取文档完整的高度
    getScrollHeight () {
      return Math.max(document.body.scrollHeight, document.documentElement.scrollHeight)
    },
    // 滚动事件触发下拉加载
    onScroll () {
      //加载数据
      if (this.getScrollHeight() - this.getClientHeight() - this.getScrollTop() <= 20) {
        if (this.status === 1) {
          this.status = 0;
          if(this.isRev && this.page>0){  //倒序
            this.page = this.page - 1 ;
            this.getInfo();
          }else if(this.page<6) {  //正序
            this.page = this.page + 1 ;
            this.getInfo();
          }
        }
      }
      //固定筛选栏
      let scrollTop = window.pageYOffset || document.documentElement.scrollTop || document.body.scrollTop;
      let offsetTop = document.querySelector('#navs').offsetTop;
      scrollTop > offsetTop ? this.isFixed = true : this.isFixed = false
    }

  },
  destroyed () {
    window.removeEventListener('scroll', this.onScroll)
  }
};



</script>

<style scoped>
  #link {
    margin-right: 10px;
    padding-top: 15px;
    height: 25px;
  }
  #link a{
    margin-right: 15px;
    float: right;
  }

  #pageTitle {
    font-size: 30px;
    color: white;
    padding-top: 10px;
    margin-bottom: 30px;
  }
  .titleCenter {
    text-align: center;
  }
  #navs {
    padding-top: 20px;
    height: 30px;
    background-color: #151d31;
  }
  .fixedBar {
    position: fixed;
    top: 0;
    z-index: 10;
    width: 100%;
  }
  #timelineBody {
    margin: 30px 230px 20px 350px;
  }
  .jnszType {
    width: 100px;
    height: 23px;
  }
  .zjhjType {
    width: 100px;
    height: 18px;
    margin-top: 2px;
  }
  .yearType {
    font-size: 18px;
    color: azure;
    border: 2px solid white;
    background: #151d31;
    width: 80px;
    height: 30px;
    border-radius: 5px;
    margin-left: 45px;
    text-align: center;
  }

  #moreInfo {
    width: 350px;
    padding-top: 50px;
    padding-right: 100px;
  }
  #foot {
    margin-top: 50px;
    padding-bottom: 30px;
    color: darkgrey;
  }
</style>

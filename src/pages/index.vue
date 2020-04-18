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
      <my-time-line :items="items">></my-time-line>
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
export default  {
  components:{
    MyNavs: navs,
    MyTimeLine
  },
  data () {
    return {
      info: [],
      showInfo: [],
      items: [],
      check: ['政府行动','境内疫情','行业战疫','境外疫情'],
      page: 1,
      status: 1,
      isFixed: false
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
      for(let i=0;i<this.info[this.page-1].length;i++){
        //不同类型不同添加不同颜色的类
        if(this.info[this.page-1][i].category === config[0].category ){
          this.info[this.page-1][i].typeColor = config[0].typeColor;
        }else if(this.info[this.page-1][i].category === config[1].category){
          this.info[this.page-1][i].typeColor = config[1].typeColor;
        }else if(this.info[this.page-1][i].category === config[2].category){
          this.info[this.page-1][i].typeColor = config[2].typeColor;
        }else if(this.info[this.page-1][i].category === config[3].category ){
          this.info[this.page-1][i].typeColor = config[3].typeColor;
        }
        //修改时间格式
        this.info[this.page-1][i].time_point = this.info[this.page-1][i].time_point.split(' ')[0];

        //判断是否是重点数据
        if(this.info[this.page-1][i].important === "0"){
          this.info[this.page-1][i].isImport = false;
        }else{
          this.info[this.page-1][i].isImport = true;
        }
        //判断是否是video
        if(this.info[this.page-1][i].video){
          this.info[this.page-1][i].isVideo = true;
        }else{
          this.info[this.page-1][i].isVideo = false;
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
    reDupTime() {   //处理数据，生成新的格式的数据
      this.items = [];
      let dateText=this.showInfo[0][0].time_point;  //时间
      let detailText = [];  //详细内容
      detailText[0]=this.showInfo[0][0];

      for(let m=0; m<this.showInfo.length;m++){
        let num=1;
        for(let i=1;i<this.showInfo[m].length;i++){
          if(this.showInfo[m][i].time_point === this.showInfo[m][i-1].time_point){
            detailText[num] = this.showInfo[m][i];
            num++;
          }else{
            this.items.push({
              date: dateText,
              detail: detailText
            });
            dateText=this.showInfo[m][i].time_point;
            num=0;
            detailText = [];
            detailText[num]=this.showInfo[m][i];
          }
          if(i === this.showInfo[m].length-1){
            this.items.push({
              date: dateText,
              detail: detailText
            });
          }
        }
      }
      this.status = 1;
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
        if (this.status === 1 && this.page<6) {
          this.status = 0;
          this.page = this.page + 1 ;
          this.getInfo();
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

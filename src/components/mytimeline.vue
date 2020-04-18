<template >
  <div class="mytimeline">
    <light-timeline :items='items'>
        <template slot='tag' slot-scope='{ item }' >
          <div class="tagText">{{item.time}}</div>
        </template>
        <template slot='symbol' slot-scope='{ item }' >
          <transition name = "fade">
            <div class="item-circle" ref="symbol" ></div>
          </transition>
        </template>
        <template slot='content' slot-scope='{ item }'>
          <div  v-for="text in item.detail" style="margin-top: 10px">
            <div class="timeLineContent"
                 ref="content"
                 :class="[text.typeColor,{'noShow':text.isShow}]"
                 @click="showPopup(text.remark,text.video)">
              <img src="../../static/images/star2.png" class="icon-important" v-show="text.isImport">
              {{text.event}}
              <img src="../../static/images/video.png" class="icon-video" v-show="text.isVideo">
            </div>
          </div>
        </template>
    </light-timeline>
    <pop-up v-show="isPopupVisible" @close="closePopup" :detailInfo="detailInfo"></pop-up>
    <pop-video v-if="isPopVideoVisible" @close="closePopVideo" :videoPath="videoPath"></pop-video>
  </div>

</template>
<script>
  import PopUp from './popup'
  import PopVideo from './popvideo'
  export default {
    name: 'mytimeline',
    components:{
      PopUp,
      PopVideo
    },
    props:['items'],
    data () {
      return {
        isPopupVisible: false,
        isPopVideoVisible: false,
        detailInfo: null,
        videoPath: null,
        contentStyle: -1
      }
    },
    mounted(){
     this.$nextTick(function () {  //监听滚动事件
        window.addEventListener('scroll', this.styleScroll)
      })
    },
    methods:{
      showPopup(remark,video){
        if(remark){
          this.detailInfo = remark;
          this.isPopupVisible = true
        }
        if(video){
          this.videoPath = video;
          //this.videoPath = "http://clips.vorwaerts-gmbh.de/big_buck_bunny.mp4";
          this.isPopVideoVisible = true
        }
      },
      closePopup(){
        this.isPopupVisible = false
      },
      closePopVideo(){
        this.isPopVideoVisible = false
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
      getClientHeight () {
        let clientHeight = 0
        if (document.body.clientHeight && document.documentElement.clientHeight) {
          clientHeight = Math.min(document.body.clientHeight, document.documentElement.clientHeight)
        } else {
          clientHeight = Math.max(document.body.clientHeight, document.documentElement.clientHeight)
        }
        return clientHeight
      },
      styleScroll() {  //监听滚动条
        //先删除上一个的样式
        if(this.contentStyle !== -1){
          this.$refs.content[this.contentStyle].classList.remove("enLarge");
          this.$refs.content[this.contentStyle].parentNode.parentNode.children[1].children[0].classList.remove("iconShape");
        }
        let high = this.getClientHeight()/2;
        let min=10000000,len,num=0;
        for(let i=0;i<this.$refs.content.length;i++){
          //console.log(i+"     获取图标  "+this.$refs.content[i].getBoundingClientRect().top)
          let len1 = this.$refs.content[i].getBoundingClientRect().top;
          len = Math.abs(len1-high);
          if(len<=min){
            min=len;
            num=i;
          }
        }
        //添加样式
        this.contentStyle = num;
        this.$refs.content[num].classList.add("enLarge");

        //获取对应的圆点
        this.$refs.content[num].parentNode.parentNode.children[1].children[0].classList.add("iconShape");
      //  console.log(this.$refs.content[num].parentNode.parentNode.children[1].firstChild)
      }
    },
    destroyed () {
      window.removeEventListener('scroll', this.styleScroll)
    }
  }
</script>
<style scoped>
  .timeLineContent{
   /* left: 120px;*/
    padding: 8px;
    font-size: 17px;
    color:#fff;
    border-radius: 5px;
    display: table-cell;
    max-width: 400px;
    text-align: left;
  }
  .mytimeline {
   /* margin-top: 50px;*/
  }
  .line-container::after {
    position: absolute;
    content: "";
    left: 5rem;
    top: 0;
    width: 2px;
    height: 100%;
    background-color: #e4e1fe;
  }
  .icon-important {
    width: 18px;
    height: 18px;
    vertical-align: top;
    margin-top: -10px;
  }
  .icon-video {
    width: 1.3em;
    height: 1.3rem;
    vertical-align: bottom;
  }
  .item-symbol {
    bottom: 10px;
  }
  .enLarge {
    transform: scale(1.1);
  }
  .iconShape {
    animation: shape 2s infinite;
  }
  @keyframes shape {
    0% {
      transform: scale(1);
    }
    25% {
      transform: scale(1.3);
    }
    50% {
      transform: scale(1);
    }
    75% {
      transform: scale(1.3);
    }
  }

  .tagText {
    font-size: 18px;
    width: 90px;
    margin-top: 12px;
    margin-left: -20px;
    color: azure;
  }
  .item-circle {
    box-sizing: border-box;
    position: absolute;
    margin-left: -7px;
    width: 16px;
    height: 16px;
    border-radius: 10px;
    background: azure !important;
    border: 4px solid #32343a !important;
    margin-top: 15px;
  }

  .noShow {
    display: none;
  }


  .inEpidemicColor {
    background: #3c8ed0;
  }
  .outEpidemicColor {
    background: #8f76c1;
    /*lightcoral*/
  }
  .actionColor {
    background:  #f77979;;
  }
  .industryColor {
    background:  #f77f46;

  }
</style>

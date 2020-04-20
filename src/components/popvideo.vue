<template>
  <div class="popup-bg">
    <div class="container">
      <div class="popup">
     <!--  <section class="popup-body">
          <video-player  class="video-player vjs-custom-skin"
                         ref="videoPlayer"
                         :playsinline="true"
                         :options="playerOptions"
          ></video-player>
        </section>-->
        <video :src=videoPath controls="controls" autoplay="autoplay"></video>
        <footer class="popup-footer">
          <slot name="footer">
            <button
              type="button"
              class="btn-green"
              @click="close">关闭
            </button>
          </slot>
        </footer>
      </div>

    </div>
  </div>

</template>

<script>
  export default {
    name: 'popvideo',
    props:['videoPath'],
/*    data () {
      return {
       playerOptions: {
//        playbackRates: [0.7, 1.0, 1.5, 2.0], //播放速度
          autoplay: false, //如果true,浏览器准备好时开始回放。
          muted: false, // 默认情况下将会消除任何音频。
          loop: false, // 导致视频一结束就重新开始。
          preload: 'auto', // 建议浏览器在<video>加载元素后是否应该开始下载视频数据。auto浏览器选择最佳行为,立即开始加载视频（如果浏览器支持）
          language: 'zh-CN',
          aspectRatio: '16:9', // 将播放器置于流畅模式，并在计算播放器的动态大小时使用该值。值应该代表一个比例 - 用冒号分隔的两个数字（例如"16:9"或"4:3"）
          fluid: true, // 当true时，Video.js player将拥有流体大小。换句话说，它将按比例缩放以适应其容器。
          sources: [{
            type: "video/ogg",
            type: "video/webm",
            type: "video/mp4",
            //src:  "https://1253125013.vod2.myqcloud.com/6cc001c1vodtranscq1253125013/1f834fd35285890797622951162/v.f20.mp4"
            src: this.videoPath
          }],
         // poster: "poster.jpg", //封面地址
          width: document.documentElement.clientWidth,
          notSupportedMessage: '此视频暂无法播放，请稍后再试', //允许覆盖Video.js无法播放媒体源时显示的默认信息。
        controlBar: {
          timeDivider: true,
          durationDisplay: true,
          remainingTimeDisplay: false,
          fullscreenToggle: true  //全屏按钮
        }
        }
      }
    },*/
  /* watch:{
      videoPath(val){
       // this.playerOptions.sources[0].src = val;
        console.log("url src   "+this.playerOptions.sources[0].src);
        //this.vPath  = val;
      }
    },*/

    methods:{
      close (){
        this.$emit('close')
      }

    }
    ,
    computed: {
     /* player() {
        return this.$refs.videoPlayer.player
      }*/
    }


  }
</script>

<style scoped>
  .popup-bg {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0,0,0,0.8);
    z-index: 20;
  }
  .container {
    position: fixed;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    background-color: rgba(0, 0, 0, 0.3);
    display: flex;
    justify-content: center;
    align-items: center;
  }
  .popup {
    background: #FFFFFF;
    box-shadow: 0.125rem 0.125rem 1.25rem 0.0625rem;
    overflow-x: auto;
    display: flex;
    flex-direction: column;
  /*  width: 40%;*/
  }
  .popup-footer{
    padding: 0.625rem 1.5625rem;
    display: flex;
  }
  .popup-footer {
    border-top: 0.0625rem solid #eeeeee;
    justify-content: flex-end;
  }
  .popup-body {
    position: relative;
    padding: 3.125rem 3.125rem;
  }
  .btn-close {
    border: none;
    font-size: 1.25rem;
    padding: 1.25rem;
    cursor: pointer;
    font-weight: bold;
    color:  #151d31;;
    background: transparent;
  }
  .btn-green {
    color: white;
    font-size: 0.8125rem;
    padding: 0.3125rem 0.625rem;
    background:  #151d31;
    border: 0.0625rem solid  #151d31;
    border-radius: 0.125rem;
  }
</style>

<template>
  <div class="popup-bg">
    <div class="container">
      <div class="popup">
        <header class="popup-header">
          <slot name="header">
            详情
            <button
              type="button"
              class="btn-close"
              @click="close">x</button>
          </slot>
        </header>
        <section class="popup-body">
          <slot name="body">
            {{detailInfo}}
          </slot>
        </section>
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
    name: 'popup',
    props:['detailInfo'],
    methods:{
      close (){
        this.$emit('close')
      }
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
    box-shadow: 2px 2px 20px 1px;
    overflow-x: auto;
    display: flex;
    flex-direction: column;
    width: 60%;
  }
  .popup-header, .popup-footer{
    padding: 10px 25px;
    display: flex;
  }
  .popup-header {
    border-bottom: 1px solid #eeeeee;
    color:  #151d31;;
    justify-content: space-between;
  }
  .popup-footer {
    border-top: 1px solid #eeeeee;
    justify-content: flex-end;
  }
  .popup-body {
    position: relative;
    padding: 20px 10px;
  }
  .btn-close {
    border: none;
    font-size: 18px;
    cursor: pointer;
    font-weight: bold;
    color:  #151d31;;
    background: transparent;
  }
  .btn-green {
    color: white;
    font-size: 13px;
    padding: 5px 10px;
    background:  #151d31;;
    border: 1px solid  #151d31;
    border-radius: 2px;
  }
</style>

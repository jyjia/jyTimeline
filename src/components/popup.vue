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
    box-shadow: 0.125rem 0.125rem 1.25rem 0.0625rem;
    overflow-x: auto;
    display: flex;
    flex-direction: column;
   /* width: 60%;*/
  }
  .popup-header, .popup-footer{
    padding: 0.625rem 1.5625rem;
    display: flex;
  }
  .popup-header {
    border-bottom: 0.0625rem solid #eeeeee;
    color:  #151d31;;
    justify-content: space-between;
  }
  .popup-footer {
    border-top: 0.0625rem solid #eeeeee;
    justify-content: flex-end;
  }
  .popup-body {
    position: relative;
    padding: 1.25rem;
    text-align: left;
  }
  .btn-close {
    border: none;
    font-size: 1.125rem;
    cursor: pointer;
    font-weight: bold;
    color:  #151d31;;
    background: transparent;
  }
  .btn-green {
    color: white;
    font-size: 0.8125rem;
    padding: 0.3125rem 0.625rem;
    background:  #151d31;;
    border: 0.0625rem solid  #151d31;
    border-radius: 0.125rem;
  }
</style>

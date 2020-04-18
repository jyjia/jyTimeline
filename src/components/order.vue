<template>
  <div class="order">
    <div class="reverse" ref="reverseShow" @click="reverseInfo()" >倒序</div>
    <div class="normal disabled" ref="normalShow" @click="normalInfo()">正序</div>
  </div>
</template>

<script>
    export default {
      name: "order",
      data() {
        return {
           isReverse: false
        }
      },
      watch: {
        isReverse() {

        }
      },
      methods: {
        reverseInfo(){
          this.isReverse = true;
          this.$emit('isReverse',this.isReverse);
          this.$refs.reverseShow.classList.add("disabled");
          this.$refs.normalShow.classList.remove("disabled");
        },
        normalInfo(){
          this.isReverse = false;
          this.$emit('isReverse',this.isReverse);
          this.$refs.normalShow.classList.add("disabled");
          this.$refs.reverseShow.classList.remove("disabled");
        }
      }
    }
</script>

<style scoped>
  .order{
    color: azure;
  }
  .normal  {
    float: right;
    border: 1px solid white;
    border-radius: 5px;
    padding: 5px;
    margin-right: 20px;
  }
  .reverse {
    float: right;
    border: 1px solid white;
    border-radius: 5px;
    padding: 5px;
  }
  .disabled {
    opacity: .2;
    pointer-events: none;
  }
</style>

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
    border: 0.0625rem solid white;
    border-radius: 0.3125rem;
    padding: 0.3125rem;
    margin-right: 1.25rem;
  }
  .reverse {
    float: right;
    border: 0.0625rem solid white;
    border-radius: 0.3125rem;
    padding: 0.3125rem;
  }
  .disabled {
    opacity: .2;
    pointer-events: none;
  }
</style>

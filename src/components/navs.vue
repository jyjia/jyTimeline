<template>
  <ul class="navs">
    <li>点击筛选 ：</li>
    <li v-for="item in navsList" class="check"
        :class="item.class"
        ref="nav"
        @click="clickCheck(item.id,item.text)">
      {{item.text}}</li>
  </ul>
</template>

<script>
  export default {
    name: "navs",
    data() {
      return {
        isCheck: [true,true,true,true],
        navsList: [
          {
            id: 0,
            class: "check0",
            text: '政府行动',
          },
          {
            id: 1,
            class: "check1",
            text: '境内疫情',
          },
          {
            id: 2,
            class: "check2",
            text: '行业战疫',
          },
          {
            id: 3,
            class: "check3",
            text: '境外疫情',
          },
        ],
        checkList: ['政府行动','境内疫情','行业战疫','境外疫情']
      }
    },
    watch:{
      checkList(){
        this.$emit('checkInfo',this.checkList);
      }
    },
    methods: {
      cancelCheck(el,text) {
        let index = this.checkList.indexOf(text);
        if (index > -1) {
          this.checkList.splice(index, 1);
          el.classList.add("disabled");   //添加不显示样式
        }
      },
      checked(el,text) {
        this.checkList.push(text);
        el.classList.remove("disabled");  //删除不显示样式
      },
      clickCheck(id,text) {
        let el = this.$refs.nav[id];
        if(this.isCheck[id]){
          //选中->没选中
          this.cancelCheck(el,text);
          this.isCheck[id] = false;
        }else{
          //没选中->选中
          this.checked(el,text);
          this.isCheck[id] = true;
        }
      }
    }
  }
</script>

<style scoped>
  .navs{
    list-style: none;
    margin:0;
    padding:0;
    color: azure;
  }
  .navs li {
    margin: 0 10px;
    display:inline-block;
  }
  .navs .check {
    transition: opacity .3s;
  }
  .navs .check:before {
    content: "";
    width: 17px;
    height: 17px;
    display: inline-block;
    border-radius: 10px;
    background: #fff;
    vertical-align: bottom;
    margin-right: 10px;
  }
  .navs .check.check0:before {
    background: #f77979;
  }
  .navs .check.check1:before {
    background: #3c8ed0;
  }
  .navs .check.check2:before {
    background: #f77f46;
  }
  .navs .check.check3:before {
    background: #8f76c1;
  }
  .check.check0{
    color: #f77979;
  }
  .check.check1{
    color: #3c8ed0;
  }
  .check.check2{
    color: #f77f46;
  }
  .check.check3{
    color: #8f76c1;
  }
  .disabled {
    opacity: .2;
  }
</style>

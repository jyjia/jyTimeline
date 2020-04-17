<template>
  <el-checkbox-group v-model="checkList">
    <el-checkbox class="item1" label="政府行动"></el-checkbox>
    <el-checkbox class="item2" label="境内疫情"></el-checkbox>
    <el-checkbox class="item3" label="行业战疫"></el-checkbox>
    <el-checkbox class="item4" label="境外疫情"></el-checkbox>
  </el-checkbox-group>
</template>

<script>
    export default {
      data() {
        return {
          checkList: ['政府行动','境内疫情','行业战疫','境外疫情']
        };
      },
      watch:{
        checkList(){
          this.$emit('checkInfo',this.checkList);
        }

      }
    };
</script>

<style>
  /*选中后框的背景和边框*/
  .item1 .el-checkbox__input.is-checked .el-checkbox__inner, .el-checkbox__input.is-indeterminate .el-checkbox__inner{
    /*background-color:#7fa037;*/
    background-color: 	#f77979;
    border-color: #f77979;
  }
  .item2 .el-checkbox__input.is-checked .el-checkbox__inner, .el-checkbox__input.is-indeterminate .el-checkbox__inner{
    background-color: #f77f46;
    border-color:#f77f46;
  }
  .item3 .el-checkbox__input.is-checked .el-checkbox__inner, .el-checkbox__input.is-indeterminate .el-checkbox__inner{
    background-color: #8f76c1;
    border-color:#8f76c1;
  }
  .item4 .el-checkbox__input.is-checked .el-checkbox__inner, .el-checkbox__input.is-indeterminate .el-checkbox__inner{
    background-color:#3c8ed0;
    border-color:#3c8ed0;
  }
  .el-checkbox__input.is-checked + .el-checkbox__label { /*选中后label*/
    color: azure;
  }
  .el-checkbox.is-bordered.is-checked{
    border-color: #909399;
  }

  /*没选中时复选框的边*/
  .item1  .el-checkbox__input.is-focus .el-checkbox__inner{
    border-color: 	#f77979;
  }
  .item2  .el-checkbox__input.is-focus .el-checkbox__inner{
    border-color: #f77f46;
  }
  .item3  .el-checkbox__input.is-focus .el-checkbox__inner{
    border-color: #8f76c1;
  }
  .item4  .el-checkbox__input.is-focus .el-checkbox__inner{
    border-color: #3c8ed0;
  }
</style>

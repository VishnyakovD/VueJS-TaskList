<template>
    <el-form :inline="true">
        <el-form-item style="width: calc(100% - 114px);">
            <el-input 
            size="large" 
            clearable 
            style="width:100%" 
            placeholder="Enter task..." 
            @input.native="textChanged" 
            @clear="textChanged"
            v-model="textCalc"></el-input>
        </el-form-item>

        <el-form-item style="margin-right:0px">
            <el-button v-if="selectedId===-1" type="primary" size="large" icon="el-icon-plus" @click="createItem" :disabled="!text">Create</el-button>
            <el-button v-else  type="primary" size="large" icon="el-icon-edit" @click="saveItem" :disabled="!text"> Save </el-button>
        </el-form-item>
    </el-form>
</template>

<script>
export default {
  name: 'InputText',
  props: {
    text: String,
    selectedId:Number
  },

   data(){
    return {
        textCalc:this.text,
    }  
  },
  
  methods:{       
      saveItem(e){
          this.$emit("textChanged",'')
      },  

      textChanged(e){
          let value=!e ? '' : e.target.value
          this.$emit("textChanged",value)
      },

      createItem(){
          this.$emit("createTask")
      }
  }
}
</script>

<style>
    .el-form-item__content{
        width: 100%;
    }
</style>



<template>
<el-row :gutter="12">
  <el-col>
    <el-card shadow="hover" v-for="(item, key) in listTaskFiltered" 
    :key="key" 
    style="margin: 10px 0;"
    :class="{selected:item.id===selectedId}"
    @click.native="selectItem(item.id, item.text)"
    >
        <div>{{item.text}}</div>
        <div>
            <br>  
            <div class="panel-item">
                {{item.date}}  
                <i class="el-icon-edit" v-if="item.isEdited"></i>
                <i :class="{'el-icon-star-on':item.isLike,'el-icon-star-off':!item.isLike}"
                @click.stop="likeItem(item.id)"></i>
                <i class="el-icon-delete" @click.stop="deleteItem(item.id)"></i>
            </div>    
        </div>
    </el-card>
  </el-col>
</el-row>
</template>

<script>
export default {
  name: 'ListTask',
  props: {
    selectedId:{default:-1},
    listTaskFiltered:{}
  },

   data(){
    return {
        textCalc:this.text,
    }  
  },
  
  methods:{
      likeItem(value){
          this.$emit("likeTask", value)
      }, 
      deleteItem(value){
          this.$emit("deleteTask", value)
      }, 
      selectItem(id,text){
          this.$emit("selectTask", {id:id,text:text})
      },      
      textChanged(e){
          this.$emit("textChanged",e.target.value)
      },
      createItem(){
          this.$emit("createTack")
      }
  }
}
</script>

<style>
    .panel-item{
        display: inline-block;
        text-align: right;
        width: 100%;
    }

.panel-item i{
        padding: 5px;
        display: inline-block;
        cursor: pointer;
        color:#409EFF;
}

    .el-form-item__content{
        width: 100%;
    }
    .selected{
        border:1px #409EFF solid;
    }
</style>



<template>
  <section>
  <div v-if="isParentTaskList">
    <h1 class="header">Task list names</h1>
    <InputText 
    :text="textName" 
    :selectedId="selectedIdName"
    @textChanged="textChangedName" 
    @createTask="createTaskName" 
    v-if="isVisibleInputName"/> 

    <ListTask 
    :listTaskFiltered="listTaskFilteredName"
    :selectedId="selectedIdName" 
     @selectTask="selectTaskName"
     @deleteTask="deleteTaskName"
     @likeTask="likeTaskName"
     @listTask1="showListTask"
      :isListNames="true"
    />
  </div>

<div v-else>
    <h1 class="header"><i class="el-icon-back" @click="isParentTaskList=!isParentTaskList"></i>{{selectedItemName.text}}</h1>
    <InputText 
    :text="text" 
    :selectedId="selectedId"
    @textChanged="textChanged" 
    @createTask="createTask" 
    v-if="isVisibleInput"/> 

    <ListTask 
    :listTaskFiltered="listTaskFiltered"
    :selectedId="selectedId" 
     @selectTask="selectTask"
     @deleteTask="deleteTask"
     @likeTask="likeTask"
    
    />
  </div>
  </section>


</template>

<script>
import Vue from 'vue'
import InputText from './InputText.vue'
import ListTask from './ListTask.vue'

export default {
  name: 'Home',
  data(){
    return {
    isParentTaskList:true,

    selectedId:-1,
    text:"",
    listTask:[],
    isVisibleInput:true,
    selectedItem:{},

    selectedIdName:-1,
    textName:"",
    listTaskName:[],
    isVisibleInputName:true,
    selectedItemName:{},
    isListNames:true
    }  
  },

  components:{
    InputText,ListTask
  },

  computed:{
    listTaskFiltered:function(){
      console.log(this.selectedItemName,"ss")
      if(this.selectedId==-1)
        return this.selectedItemName.listTask.filter(item=>{
          if(item.text.indexOf(this.text)===0) return item
        })
      else
        return this.listTask.filter(item=>{
          if(item.id===this.selectedId) return item
        })
    },

    listTaskFilteredName:function(){
      if(this.selectedIdName==-1)
        return this.listTaskName.filter(item=>{
          if(item.text.indexOf(this.textName)===0) return item
        })
      else
        return this.listTaskName.filter(item=>{
          if(item.id===this.selectedIdName) return item
        })
    }
  },

  updated(){
      Vue.nextTick(()=>{
        this.isVisibleInput=true
        this.isVisibleInputName=true
      })
  },

  methods:{    
    likeTask(value){
      this.selectedItemName.listTask.find(item=>{
        if(item.id===value) item.isLike=!item.isLike
      })
      
    },

    likeTaskName(value){
      this.listTaskName.find(item=>{
        if(item.id===value) item.isLike=!item.isLike
      })      
    },

    deleteTask(value){
      let idx=this.selectedItemName.listTask.findIndex(item=>{
        if(item.id===value) return true
      })
      
      this.selectedItemName.listTask.splice(idx,1)
    },

    deleteTaskName(value){
      let idx=this.listTaskName.findIndex(item=>{
        if(item.id===value) return true
      })
      
      this.listTaskName.splice(idx,1)
    },

    selectTask(item){
      this.isVisibleInput=false
      if(this.selectedId===item.id){
        this.selectedId=-1
        this.text=""
      }
      else{
        
        this.selectedId=item.id
        this.text=item.text
        this.selectedItem=this.selectedItemName.listTask.find(elem=>{
        if(elem.id===item.id) return true
      })
      console.log("selectedItemName", this.selectedItemName.listTask)
      console.log("selectedItem----", this.selectedItem)
      }      
    },

    selectTaskName(item){
      this.isVisibleInputName=false
      if(this.selectedIdName===item.id){
        this.selectedIdName=-1
        this.textName=""
      }
      else{
        this.selectedIdName=item.id
        this.textName=item.text
        this.selectedItemName=this.listTaskName.find(elem=>{
        if(elem.id===item.id) return true
      })
      }      
    },

    textChanged(value){
      this.text=value

      if(this.selectedId>-1 && value!==''){
         let dateCreate=new Date()
        this.selectedItem.text=value
        this.selectedItem.date=`${dateCreate.toLocaleDateString()} ${dateCreate.toLocaleTimeString()}`
        this.selectedItem.isEdited=true
      } 
    else{
        this.selectedId=-1
      }  
    }, 

    textChangedName(value){
      this.textName=value

      if(this.selectedIdName>-1 && value!==''){
        let dateCreate=new Date()
        this.selectedItemName.text=value
        this.selectedItemName.date=`${dateCreate.toLocaleDateString()} ${dateCreate.toLocaleTimeString()}`
        this.selectedItemName.isEdited=true
      } 
    else{
        this.selectedIdName=-1
      }  
    }, 

    createTask(){
      let dateCreate=new Date()
      this.selectedItemName.listTask.push(
        {        
          id:dateCreate.getTime(),
          text:this.text,
          date:`${dateCreate.toLocaleDateString()} ${dateCreate.toLocaleTimeString()}`,
          isLike:false,
          isEdited:false,
        }
      )
      this.isVisibleInput=false
      this.text=""
      this.selectedId=-1  
      Vue.nextTick(()=>{
         this.isVisibleInput=true
      })    
    },

    createTaskName(){

      let dateCreate=new Date()
      this.listTaskName.push(
        {        
          id:dateCreate.getTime(),
          text:this.textName,
          date:`${dateCreate.toLocaleDateString()} ${dateCreate.toLocaleTimeString()}`,
          isLike:false,
          isEdited:false,
          listTask:[]
        }
      )

      this.isVisibleInputName=false
      this.textName=""
      this.selectedIdName=-1  
      Vue.nextTick(()=>{
         this.isVisibleInputName=true
      })    
    },
    showListTask(){
      //=
      this.isParentTaskList=!this.isParentTaskList
      

      console.log("xxxx", this.selectedItemName, this.selectedIdName)
    }
  }
}
</script>

<style>
.header {
    text-align: center;
    margin: 55px 0 20px;
    font-weight: 400;
    color: #1f2f3d;
    margin-block-start: 1em;
    margin-block-end: 1em;
    margin-inline-start: 0px;
    margin-inline-end: 0px;
    font-family: Helvetica Neue,Helvetica,PingFang SC,Hiragino Sans GB,Microsoft YaHei,SimSun,sans-serif;
    -webkit-font-smoothing: antialiased;
}
</style>




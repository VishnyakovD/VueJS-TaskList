<template>
  <div>
    <h1 class="header">Task list</h1>
    <InputText 
    :text="text" 
    :selectedId="selectedId"
    @textChanged="textChanged" 
    @createTack="createTask" 
    v-if="isVisibleInput"/> 

    <ListTask 
    :listTaskFiltered="listTaskFiltered"
    :selectedId="selectedId" 
     @selectTask="selectTask"
     @deleteTask="deleteTask"
     @likeTask="likeTask"
    />
  </div>
</template>

<script>
import Vue from 'vue'
import InputText from './InputText.vue'
import ListTask from './ListTask.vue'

export default {
  name: 'Home',
  data(){
    return {
    selectedId:-1,
    text:"",
    listTask:[],
    isVisibleInput:true,
    selectedItem:{}
    }  
  },

  components:{
    InputText,ListTask
  },

  computed:{
    listTaskFiltered:function(){
      if(this.selectedId==-1)
        return this.listTask.filter(item=>{
          if(item.text.indexOf(this.text)===0) return item
        })
      else
        return this.listTask.filter(item=>{
          if(item.id===this.selectedId) return item
        })
    }
  },

  updated(){
          Vue.nextTick(()=>{
         this.isVisibleInput=true
      })
  },

  methods:{    
    likeTask(value){
      this.listTask.find(item=>{
        if(item.id===value) item.isLike=!item.isLike
      })
      
    },
    deleteTask(value){
      let idx=this.listTask.findIndex(item=>{
        if(item.id===value) return true
      })
      
      this.listTask.splice(idx,1)
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
        this.selectedItem=this.listTask.find(elem=>{
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

    createTask(){
      let dateCreate=new Date()
      this.listTask.push(
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




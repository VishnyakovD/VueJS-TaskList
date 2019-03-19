<template>
  <el-row :gutter="20" id="app">
   <el-col :span="12" :offset="6">   
       <Home/>
    </el-col>
  </el-row>
</template>

<script>
import Home from './components/Home.vue'

export default {
  name: 'app',
  components: {
    Home
  }
}
</script>

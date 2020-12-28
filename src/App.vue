<template>
  <div id="app">
    <img alt="拓补图" src="./assets/Topography.png">
    <HelloWorld msg="Welcome to Your Vue.js App"/>

    <label for="input_ip_r0">IP地址</label><input id="input_ip_r0" type="text" v-model="ip_r0">
    <label for="input_pwd_telnet_r0">Telnet密码</label><input id="input_pwd_telnet_r0" type="text" v-model="pwd_telnet_r0">
    <label for="input_pwd_enable_r0">特权密码</label><input id="input_pwd_enable_r0" type="text" v-model="pwd_enable_r0">
    <button v-on:click="test">TEST</button>
  </div>
</template>


<script>
import HelloWorld from './components/HelloWorld.vue'
import axios from 'axios'

export default {
  name: 'App',
  components: {
    HelloWorld
  },
  data(){
    return{
      ip_r0:'',
      pwd_telnet_r0:'',
      pwd_enable_r0:''
    }
  },
  methods:{
    //前端调用测试
    test (){
      const url='http://127.0.0.1:5000/test'
      let params={
        ip:'1.1.1.1',
        pwd:'12345'
      }
      axios({
        method: 'post',
        url: url,
        data: params
      }).then(res => {
        console.log(res)
      }).catch(err => {
        console.log(err)
      })
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>

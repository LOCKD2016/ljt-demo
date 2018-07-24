<template>
  <div id="app">
    <input type="text" name="" placeholder="出发城市" v-model='city' @focus='showIt'>
    <template v-if='status'>
      <HelloWorld v-on:choose='getCity' :msg='city'/>
    </template>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld'

export default {
  name: 'App',
  data(){
    return{
      status:false,
      city:''
    }
  },
  components: {
    HelloWorld
  },
  methods:{
    showIt(){
      this.status = true;
    },
    hideIt(){
      this.status = false;
    },
    getCity(val){
      console.log(val);
      this.status = false;
      this.city = val;
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /*text-align: center;*/
  color: #2c3e50;
  margin-top: 60px;
}
</style>

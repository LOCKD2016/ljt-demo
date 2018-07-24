<template>
  <div class="wrap">
    <template v-if='searchstatus'>
      <ul class="search">
        <li v-for='(res,index) in result' :key='index' @click='chooseCity(res)'>{{ res }}</li>
      </ul>
    </template>
    <template v-else>
      <ul class="codes">
        <li :class='is_active == item ? "active" : "" ' v-for='(item,index) in codes' :key='index' @click='changeCity(item)'>{{ item }}</li>
      </ul>
      <ul class="citys">
        <li v-for='(item,index) in currentdata' :key='index'>
          <span>{{ item.code }}</span>
          <span class="city" v-for='(city,index) in item.name' :key='index' @click='chooseCity(city)'>{{ city }}</span>
        </li>
      </ul>
    </template>
  </div>
</template>

<script type="text/javascript">

import Vue from 'vue';
import city from '../assets/city.json'
export default{
  data(){
    return{
      codes:['ABCDE','FGHJ','KLMNP','QRSTW','XYZ'],
      alldata:{},
      currentdata:{},
      citydata:city,
      is_active:'ABCDE',
      searchstatus:false,
      result:[],
      zimu:['A','B','C','D','E','F','G','H','J','K','L','M','N','P','Q','R','S','T','W','X','Y','Z']
    }
  },
  created(){
    for(let i in this.citydata){
      let tem = this.firstcode(i);
      for(let j = 0; j < this.zimu.length;j++){
        if(tem == this.zimu[j]){
          if(this.alldata[this.zimu[j]]){
            this.alldata[this.zimu[j]].push(this.citydata[i]);
          }else{
            this.alldata[this.zimu[j]] = [];
            this.alldata[this.zimu[j]].push(this.citydata[i]);
          }
        }
      }
    }
    this.getCitys('ABCDE');
    console.log(this.alldata);
  },
  props:['msg'],
  watch:{
    msg(val){
      if(val == ''){
        this.searchstatus = false;
      }else{
        this.searchstatus = true;
      }
      this.result = [];
      for(var i in this.citydata){
        if(this.citydata[i].indexOf(val)>-1 && val!=''){
          this.result.push(this.citydata[i]);
        }
      }
    }
  },
  methods:{
    firstcode(val){
      return val.slice(0,1).toUpperCase();
    },
    changeCity(val){
      switch(val){
        case 'ABCDE':
          this.getCitys('ABCDE');
          break;
        case 'FGHJ':
          this.getCitys('FGHJ');
          break;
        case 'KLMNP':
          this.getCitys('KLMNP');
          break;
        case 'QRSTW':
          this.getCitys('QRSTW');
          break;
        case 'XYZ':
          this.getCitys('XYZ');
          break;
      }
    },
    getCitys(val){
      this.currentdata = [];
      this.is_active = val;
      for(let i = 0 ; i < this.zimu.length;i++){
        if(val.indexOf(this.zimu[i])>-1){
          var obj = {
            code:this.zimu[i],
            name:this.alldata[this.zimu[i]]
          };
          this.currentdata.push(obj);
          console.log(this.currentdata);
        }
      }
    },
    chooseCity(val){
      this.$emit('choose',val);
      this.searchstatus = false;
    }
  }
}

</script>

<style type="text/css">
.wrap{
  width: 420px;
  height: 250px;
}
.codes{
  width: 100%;
  height:30px;
  overflow: hidden;
  margin: 0;
  padding: 0;
  list-style: none;
}
.codes li{
  float: left;
  padding: 6px 0;
  margin-right: 10px;
  cursor: pointer;
}
.active{
  color: red;
}
.citys{
  width: 100%;
  overflow: hidden;
  margin: 0;
  padding: 0;
  list-style: none;
}
.city{
  padding: 0 6px;
  margin-right: 6px;
  cursor: pointer;
}
.search{
  width: 100%;
  overflow: hidden;
  margin: 0;
  padding: 0;
  list-style: none;
}
.search li{
  cursor: pointer;
}
.search li:hover{
  background: skyblue;
}
</style>

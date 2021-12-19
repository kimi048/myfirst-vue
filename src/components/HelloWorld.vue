<template>
  <!-- <img alt="Vue logo" src="./assets/logo.png">
  <HelloWorld msg="Welcome to Your Vue.js App2"/> -->
  <div>
    <p>
      {{msg}}
    </p>
    <input type="text" v-model="msg"><br>
    <p>name</p>
    <input type="text" v-model="name"><br>
    <p>screen_name</p>
    <input type="text" v-model="screen_name"><br>
    <p>bio</p>
    <input type="text" v-model="bio"><br>
    <button type="button" @click="sendtoApi()">送信</button>
  </div>
</template>

<script>
//import HelloWorld from './components/HelloWorld.vue'
import axios from "axios";
export default {
  name: 'App',
  components: {
    //HelloWorld
  },
  data () {
    return {
      msg: 'Hello World!',
      users:"",
      name:"",
      screen_name:"",
      bio:""
    }
  },created() {
    console.log("hogehoge");
    axios.defaults.headers.post['Content-Type'] = 'application/json;charset=utf-8';
    axios.defaults.headers.post['Access-Control-Allow-Origin'] = '*';
    axios.get('http://localhost:3000/api/v1/user/')
          .then(function(response) {
             
             console.log(response.data);
          })
           .catch(function(error){
            console.log(error)
          })
          
  },mounted: function(){
    axios.get('https://api.coindesk.com/v1/bpi/currentprice.json')
    .then(function(response){
        //デバッグ用にconsoleに出力
        console.log(response.data.bpi)
        this.bpi = response.data.bpi
    }.bind(this))
    .catch(function(error){
        console.log(error)
    })
  },
  methods:{
    sendtoApi:function(){
      axios.post('http://localhost:3000/api/v1/user/',{
        name:this.name,
        screen_name:this.screen_name,
        bio:this.bio
      }).then((res) => {
        console.log(res);
        this.posts = res.data.posts;
      })
      .catch((err) => {
        console.log(err);
      });
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
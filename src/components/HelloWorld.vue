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
    <p>delete id</p>
    <input type="text" v-model="deleteid"><br>
    <button type="button" @click="deletetoApi(deleteid)">送信</button>
  </div>
  <div v-for="user2 in users2" v-bind:key="user2.name">
    {{user2._id}}
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
      bio:"",
      deleteid:"",
      users2:[]
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
    axios.get('http://localhost:3000/api/v1/user/')
    .then(function(response){
        //デバッグ用にconsoleに出力
        console.log("bf");
        console.log(response.data);
        console.log("af")
        this.users2 = response.data;
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
        this.name="";
        this.screen_name="";
        this.bio="";
      })
      .catch((err) => {
        console.log(err);
      });
    },
    deletetoApi:function(id){
      console.log("trying to delete");
      axios.delete('http://localhost:3000/api/v1/user/'+id)
      .then(response => {
        console.log(response);
        this.deleteid="";
      })
      .catch(error => console.log(error))
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
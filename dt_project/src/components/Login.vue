<template>
<img alt="Vue logo" src="../assets/logo_dt.png">

<h1>
    Login
</h1> 
<br/>
<br/>
<br/>
<br/>
<div class="login" id="up">
    <input type="text" v-model="username" placeholder="Enter Name" />
    <input type="password" v-model="password" placeholder="Enter Password" />
    <button v-on:click="login">Login</button>
    <p>
    <router-link to="/sign-up">Sign Up</router-link>
    </p>
</div>
</template>

<script>
import axios from 'axios'
export default {
    //eslint-disable-next-line
    name:'Login',
    data()
    {
        return {
            username:'',
            password:'',
        }
    },
    methods:{
        async login()
        {   
            let result = await axios.get(
                `http://localhost:3000/user?username=${this.username}&password=${this.password}`
            ) 

             if(result.status==200 && result.data.length>0)
            {
                localStorage.setItem("user-info", JSON.stringify(result.data[0]))
                this.$router.push({name:'Home'})
            }
            console.warn(result)
        } 

    },
    mounted(){
        let user = localStorage.getItem('user-info');
        if(user){
            this.$router.push({name:'Home'})
        }
    }
}
</script>
<style>
#up {
  align-items: center;
  border:10px;
  border-color: rgb(191, 0, 255);
  border-radius: 5px;
  background-color: #757575;
  width:30%;
  height:60%;
  padding:50px;
  margin:auto;
}
</style>
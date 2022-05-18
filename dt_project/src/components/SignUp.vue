<template>
<h1>
    Sign Up
</h1> 
<div class="register">
    <input type="text" v-model="username" placeholder="Enter Name" />
    <input type="password" v-model="password" placeholder="Enter Password" />
    <button v-on:click = "signUp">Sign Up</button>
</div>
</template>

<script>
import axios from 'axios'
export default{
    name: 'SignUp',
    data()
    {
        return{
            username: '',
            password: ''
        }
    },

    methods:
    {
       async signUp()
       {
           let result = await axios.post("http://localhost:3000/user",{
               username:this.username,
               password:this.password
           });

           console.warn(result);
           if(result.status==201)
           {
               alert("sign up done");
               localStorage.setItem("user-info", JSON.stringify(result.data))
           }
       }
    }

}
</script> 
<style>
.logo{
    width:100px
} 
.register input{ 
    width: 300px;
    height: 40px;
    padding-left: 20px;
    display: block;
    margin-bottom: 30px;
    margin-right: auto;
    margin-left: auto;
    border: 1px solid skyblue;

} 
.register button{
    width: 320px;
    height: 40px;
    border: 1px solid skyblue;
    background: steelblue;
    color: #fff;
    cursor: pointer;
}
</style>
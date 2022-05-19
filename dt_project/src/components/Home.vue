<template>
<Header />
<h1>
    Hello {{username}}, Welcome on Home page
</h1>
<table border="1">
    <tr>
        <td>Id</td>
        <td>Name</td>
        <td>Date</td>
        <td>Time</td>

    </tr>
    <tr v-for="item in show" :key="item.id">
        <td>{{item.id}}</td>
        <td>{{item.name}}</td>
        <td>{{item.date}}</td>
        <td>{{item.time}}</td>
    </tr>
</table>
</template>
<script>

import Header from './Header.vue'
import axios from 'axios';

export default {
    //eslint-disable-next-line
    name: 'Home', 
    data(){
        return {
            username:'',
            show:[],
        }
    },
    components:{
        Header
    },
      async mounted(){
        let user = localStorage.getItem('user-info');
        this.username = JSON.parse(user).username;
        if(!user){
            this.$router.push({name:'SignUp'})
        }
        let result = await axios.get("http://localhost:3000/show");
        console.warn(result)
        this.show = result.data;
    }
    
}
</script>

<style scoped>
    td{
        width:160px;
        height: 50;
    }
</style>
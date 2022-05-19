<template>
<Header />
<h1>
    Hello {{username}}, Welcome on Home page
</h1>
<table>
    <tr v-for="item in show" :key="item.id">
        <td>{{item.name}}</td>
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
      mounted(){
        let user = localStorage.getItem('user-info');
        this.username = JSON.parse(user).username;
        if(!user){
            this.$router.push({name:'SignUp'})
        }
        let result = axios.get("http://localhost:3000/show");
        console.warn(result)
        this.show = result.data;
    }
    
}
</script>
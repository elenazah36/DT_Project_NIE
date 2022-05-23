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
        <td v-show="isAdmin == true">Actions</td>

    </tr>
    <tr v-for="item in show" :key="item.id">
        <td>{{item.id}}</td>
        <td>{{item.name}}</td>
        <td>{{item.date}}</td>
        <td>{{item.time}}</td>
        <td v-show="isAdmin == true">
            <router-link :to="'/update/'+item.id">Update</router-link>
            <button v-on:click="deleteShow(item.id)">Delete</button>
        </td>
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
            isAdmin:'',
            username:'',
            show:[],
        }
    },
    components:{
        Header
    },
    methods:{
        async deleteShow(id){
            let result = await axios.delete("http://localhost:3000/show/"+id);
            console.warn(result)
            if(result.status==200){
                this.loadData();
            }
        },

        async loadData(){
            let user = localStorage.getItem('user-info');
            this.username = JSON.parse(user).username;
            this.isAdmin = JSON.parse(user).admin;
            if(!user){
                this.$router.push({name:'SignUp'})
            }
            let result = await axios.get("http://localhost:3000/show");
            console.warn(result)
            this.show = result.data;
        }
    },
      async mounted(){
        this.loadData();
    }
    
}
</script>

<style scoped>
    td{
        width:160px;
        height: 50;
    }
</style>
<template>
<Header />
<h1>
    Hello {{username}}, add a show! Let's SHOW them who's boss! 
</h1>
<!--Show add form-->
<form class="add">
    <input type="text" name="name" placeholder="Enter name" v-model="show.name"/>
    <input type="date" name="date" placeholder="Enter date" v-model="show.date"/>
    <input type="time" name="time" placeholder="Enter starting time" v-model="show.time"/>
    <button type="button" v-on:click="addShow">Add new show</button>
    
</form>

</template>
<script>
import Header from './Header.vue';
import axios from 'axios';
export default {
    //eslint-disable-next-line
    name: 'Add', 
    components:{
        Header
    },
    data(){
        return {
            username:'',
            show:{
                name:'',
                date:'',
                time:'',
            }
        }
    },
    methods:{
        async addShow(){
            console.warn(this.show);
            const result = await axios.post("http://localhost:3000/show",{
                name:this.show.name,
                date:this.show.date,
                time:this.show.time,
            });
            if(result.status==201){
                this.$router.push({name:'Home'})
            }
            console.warn("result",result)
        }
    },
      mounted(){
        let user = localStorage.getItem('user-info');
        this.username = JSON.parse(user).username;
        if(!user){
            this.$router.push({name:'SignUp'})
        }
    }
    
}
</script>
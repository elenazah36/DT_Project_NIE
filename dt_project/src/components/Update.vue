<template>
<Header/>
<br/>
<br/>
<h1>
    Hello {{username}}, need replanning? Let's update the thing!
</h1>

<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<!--Show add form-->

<form class="add" id="up">
    <input type="text" name="name" placeholder="Enter name" v-model="show.name"/>
    <input type="date" name="date" placeholder="Enter date" v-model="show.date"/>
    <input type="time" name="time" placeholder="Enter starting time" v-model="show.time"/>
    <button type="button" id="button" v-on:click="updateShow">Update show</button>
    
</form>
</template>
<script>
import Header from './Header.vue';
import axios from 'axios';
export default {
    //eslint-disable-next-line
    name: 'Update', 
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
        async updateShow(){
            console.warn(this.show);
            const result = await axios.put("http://localhost:3000/show/"+this.$route.params.id,{
                name:this.show.name,
                date:this.show.date,
                time:this.show.time,
            });
            if(result.status==200){
                this.$router.push({name:'Home'})
            }
            console.warn("result",result)
        }
    },
      async mounted(){
        let user = localStorage.getItem('user-info');
        this.username = JSON.parse(user).username;
        let isAdmin = JSON.parse(user).admin;

        if(!user){
            this.$router.push({name:'SignUp'})
        }
        if(!isAdmin){
            this.$router.push({name:'Home'})
        }

        // get the show from the db via ID in link
        const result = await axios.get('http://localhost:3000/show/'+this.$route.params.id)
        
        // put the result from the server into the show var
        this.show = result.data;
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

#button{
    background-color: black;
}
</style>
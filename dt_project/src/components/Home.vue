<template>
<Header />


<h1>
    Hello {{username}}, Welcome on Home page
</h1>

<br/>
<br/>
<br/>
<br/>


<body>
<table>
    <tr background-color=#095d22 >
        <td>Id</td>
        <td>Name</td>
        <td>Date</td>
        <td>Time</td>
        <td v-show="isAdmin == true">Actions</td>
        <td v-show="isAdmin == null">User Actions</td>
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
        <td  v-show="isAdmin == null">
            <button v-on:click="bookShow(item.id)">Book Show</button>
        </td>
    </tr>
</table>
</body> 

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
            booking:[],
        }
    },
    components:{
        Header
    },
    methods:{

        async bookShow(showid){
            console.log(showid);
            let user = localStorage.getItem('user-info'); 
            //get logged in user for userid
            const result = await axios.post("http://localhost:3000/booking",{
                     showid:showid,  
                     userid:JSON.parse(user).id,
                      
            });
            //show id is passed as parameter 
             if(result.status==201){
                 this.$router.push({name:'History'})
             }
             },

            //create booking or userid and showid
    

        async deleteShow(id){
            // delete show
            let resultShow = await axios.delete("http://localhost:3000/show/"+id);

            // delete bookings associated with the show
            let getBooking = await axios.get("http://localhost:3000/booking?showid_like=["+id+"]");
            console.warn(getBooking);

           //eslint-disable-next-line
            getBooking.data.forEach(async function(item,index){
                 let deleteBooking = axios.delete("http://localhost:3000/booking/"+item.id);
                 console.log(deleteBooking);
             })
            console.log(getBooking)
            if(resultShow.status==200 && getBooking.status==200){
                this.loadData();
            }
            this.loadData();
        },

        async loadData(){

            //get user info
            let user = localStorage.getItem('user-info');
            this.username = JSON.parse(user).username;
            this.isAdmin = JSON.parse(user).admin;
            if(!user){
                this.$router.push({name:'SignUp'})
            }

            //get shows
            let showsResult = await axios.get("http://localhost:3000/show");
            this.show = showsResult.data;
            
            //get bookings for the logged-in user
            let bookingsResult = await axios.get("http://localhost:3000/booking?userid="+user.id); 
            this.booking = bookingsResult.data;
            console.warn(this.show);
        }
    },
      async mounted(){ 
         this.loadData();
    }
    
}
</script>

<style>

body  {
  background-image: url(https://images.unsplash.com/photo-1556340346-5e30da977c4d?ixlib=rb-1.2.1&raw_url=true&q=80&fm=jpg&crop=entropy&cs=tinysrgb&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=871);
  background-color: #060503;
  background-size:cover;
  height: 100vh;
  width: 100vw;
  background-position: right top;
} 

table{
  background-color:rgb(194, 194, 194);
  border-style:solid;
  border-color: #c2bfb5;
  border-radius: 10px;
  padding:5px;
  margin:auto;
  width:70%;
}

th, td {
  text-align: left;
  padding: 8px;
}

tr:nth-child(even) {
  background-color: #8f8f8f;
}

h1{
   color:beige; 
}

router{
border: 2px solid red;
border-radius: 12px;
padding: 5px;
}

button{
   background-color: rgb(58, 59, 59); 
   border-radius: 12px;
   color:ivory;
   cursor: pointer;
   margin:1px;
}
router-link:link{
    color: grey;
} 
route-link:visited{
    color:pink
} 
router-link:hover{
    color: red;
} 
router-link:active{
    color:yellow;
}
</style>

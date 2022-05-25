<template>
    
<Header />
<h1>
    Hello {{username}}, This is your booking history
</h1>

<br/>
<br/>
<br/>
<br/>
<table border="1">
    <tr>
        <td>Id</td>
        <td>Show Name</td>
        <td>Date</td>
        <td>Time</td>
        <td>Actions</td>

    </tr>
    <tr v-for="item in showsHistory" :key="item.id">
        <td>{{item.id}}</td>
        <td>{{item.name}}</td>
        <td>{{item.date}}</td>
        <td>{{item.time}}</td>
        <td >
            <button v-on:click="deleteBooking(item.id)">Delete</button>
        </td>
    </tr>
</table>
</template>

<script>
import Header from './Header.vue'
import axios from 'axios';

export default{
    //eslint-disable-next-line
    name:'History',
    data(){
        return {
            username:'',
            bookings:[],
            showsHistory:[],
        }
    },
    components:{
        Header
    },
    methods:{
       async deleteBooking(id){
            //get user for their id
            let user = localStorage.getItem('user-info');
            
            let resultSearchBooking = await axios.get("http://localhost:3000/booking/?userid="+JSON.parse(user).id+"&showid="+id);
            console.log(resultSearchBooking);
            let bookingValue = resultSearchBooking.data;
            console.log(bookingValue);
            let resultDeleteBooking = await axios.delete("http://localhost:3000/booking/"+bookingValue[0].id);
            if(resultDeleteBooking.status==200){
                this.loadData();
            }
            this.loadData();
        },

        //used to display bookings for the logged-in user
        async loadData(){
            //get user for their id
            let user = localStorage.getItem('user-info');
            let userid = JSON.parse(user).id;
            if(!user){
                this.$router.push({name:'SignUp'})
            }
            //find bookings with userid == our user's id
            let result = await axios.get("http://localhost:3000/booking?userid="+userid);
            this.bookings = result.data;
            
            //store all shows' indexes in
            let showsaux=new Array();
            //eslint-disable-next-line
             result.data.forEach(async function(item,index){
                 showsaux.push(item.showid);
             })

            //get shows that the user booked
            let showsresult = await axios.get("http://localhost:3000/show?id_like=["+showsaux.toString()+"]");
            console.log("http://localhost:3000/show?id_like=["+showsaux.toString()+"]")
            //send the shows in the table
            this.showsHistory = showsresult.data;
            console.log(this.showsHistory);
        },
    },
      async mounted(){
        this.loadData();
    }
}
</script>

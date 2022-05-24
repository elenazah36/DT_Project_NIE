<template>
    
<Header />
<h1>
    Hello {{username}}, This is your booking history
</h1>
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
            //find booking with id==id
            let bookingResult = await axios.get("http://localhost:3000/booking?userid="+user.id);
            //find show
            let showResult = await axios.get("http://localhost:3000/show?id="+bookingResult.data.showid);
            //seat count ++
            let resultUpdateShow = await axios.put("http://localhost:3000/booking/"+id,{
                seats: showResult.seats+1,
            });
            
            
            
            let resultDeleteBooking = await axios.delete("http://localhost:3000/booking/"+id);
            if(resultDeleteBooking.status==200 && resultUpdateShow.status==200){
                this.loadData();
            }
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
            
            let showsaux=new Array();
            let str ='';
            //eslint-disable-next-line
             result.data.forEach(async function(item,index){
                str = str + ',' + item.showid
            //     let itemid = item.showid;
            //     let searchShow = await axios.get("http://localhost:3000/show/"+itemid);
                 showsaux.push(item.showid);
             })

            str.substring(1);
            

            let showsresult = await axios.get("http://localhost:3000/show?id_like=["+showsaux.toString()+"]");
            console.log("http://localhost:3000/show?id_like=["+showsaux.toString()+"]")
            this.showsHistory = showsresult.data;
            console.log(this.showsHistory);
        },
    },
      async mounted(){
        this.loadData();
    }
}
</script>

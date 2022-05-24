<template>
   <div class="nav">
       <router-link to="/">Home</router-link>
       <router-link to="/add" v-show="isAdmin==true">Add Show</router-link>
       <!--<router-link to="/update" v-show="isAdmin==true">Update Show</router-link>-->
       <a v-on:click="logout" href="#">Logout</a>
       <p> {{username}}</p>
   </div>
   
</template>

<script>

export default {
    //eslint-disable-next-line
    name:'Header',
    data(){
        return{
            username:'',
            isAdmin:''
        }
    },
    methods:{
        logout(){
            localStorage.clear();
            this.$router.push({name:'Login'})
            
        },
        async loadData(){
            let user = localStorage.getItem('user-info');
            this.username = JSON.parse(user).username;
            this.isAdmin = JSON.parse(user).admin;
            if(!user){
                this.$router.push({name:'SignUp'})
            }
        }
    },
    
      async mounted(){
        this.loadData();
    }
}
</script>



<style scoped>
/* Navbar style basic color */
.nav{
    background-color:#333;
    overflow: hidden;
}
/* Navbar anchors style */
.nav a{
    float:left;
    color: #f2f2f2;
    padding:16px 14px;
    text-align: center;
    font-size: 17 px;
    text-decoration: none;
    margin-right:5px;
}
/* Navbar anchors style when hovering */
.nav a:hover{
    background-color: #ddd;
    color:aqua
}  
.nav p{
   color:antiquewhite;
   text-align: right;
   margin-right:15px;
}
</style>
<template>
   <div class="nav">
       <a href="#">Home</a>
       <a href="#">Add Show</a>
       <a href="#">Update Show</a>
       <a v-on:click="logout" href="#">Logout</a>
   </div>
</template>

<script>

export default {
    //eslint-disable-next-line
    name:'Header',
    methods:{
        logout(){
            localStorage.clear();
            this.$router.push({name:'Login'})
        }
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
</style>
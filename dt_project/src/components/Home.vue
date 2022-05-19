<template>
<Header />
<h1>
    Hello User, Welcome on Home page
</h1>
</template>
<script>
import Header from './Header.vue'
export default {
    //eslint-disable-next-line
    name: 'Home', 
    components:{
        Header
    },
      mounted(){
        let user = localStorage.getItem('user-info');
        if(!user){
            this.$router.push({name:'SignUp'})
        }
    }
    
}
</script>
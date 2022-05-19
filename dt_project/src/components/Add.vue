<template>
<Header />
<h1>
    Hello User, add a show! Let's SHOW them who's boss! 
</h1>
</template>
<script>
import Header from './Header.vue'
export default {
    //eslint-disable-next-line
    name: 'Add', 
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
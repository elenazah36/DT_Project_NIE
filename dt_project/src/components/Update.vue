<template>
<Header />
<h1>
    Hello User, need replanning? Let's update the thing!
</h1>
</template>
<script>
import Header from './Header.vue'
export default {
    //eslint-disable-next-line
    name: 'Update', 
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
<template>
  <v-app >
    <router-view/>
  </v-app>
</template>

<script>
import footMe from './components/footMe';

export default {
  name: 'App',
  components: {
  },
  data: () => ({
 
  }),
  created(){
    if(!localStorage.getItem('access_token')){
      this.$router.push('/home').catch(()=>{})
    }
  }
};
</script>

<style>
</style>

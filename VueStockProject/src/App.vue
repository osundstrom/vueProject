<script>
import { RouterLink, RouterView } from 'vue-router';
import navigation from './components/navigation.vue'


export default {
    components: {navigation}
  }

</script>

<template>
  
  <navigation/>

  <RouterView/>



</template>

<style scoped>
  div{
    overflow-x: hidden;
  }
</style>


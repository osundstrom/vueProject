
<script>

  export default {
    name: "navigation",
    components: {
      RouterLink
    }
  }

  import { RouterLink } from 'vue-router';

</script>




<template>



<ul>
  <li><RouterLink  to="/">Login</RouterLink></li>
  <li><RouterLink  to="/stock">Stock</RouterLink></li>
  
</ul>



</template>




<style scoped>
  ul{
    color: black;
    list-style: none;
    text-align: center;
  }
  li{
    display: inline-block;
    margin-right: 2vw;
    font-size: larger;
    font-weight: 900;
  }
  a{
    text-decoration: none;
    color: black;
  }
  a:hover{
    color: white;
  }
    

</style>

<script>

  export default {
    name: "navigation",
    components: {
      RouterLink
    },

    methods: {
      logout() {
        localStorage.removeItem("token");
        console.log("utloggad")
      }
    }
  }

  import { RouterLink } from 'vue-router';

</script>




<template>



<ul>
  <li>
    <RouterLink  to="/" @click="logout">Logga ut</RouterLink>
  </li>
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
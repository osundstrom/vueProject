
<script>

  export default {
    name: "navigation",
    components: {
      RouterLink 
    },

    methods: {
      //funktion utlogg
      logout() {
        localStorage.removeItem("token"); //tar bort token.
        console.log("utloggad")
      }
    }
  }
//importerar routerlink
  import { RouterLink } from 'vue-router';

</script>




<template>



<ul>
  <li>
     <!-- Knapp för utlogg-->
    <button type="button" class="btn btn-danger"><RouterLink  to="/" @click="logout">Logga ut</RouterLink></button>
  </li>
</ul>



</template>




<style scoped>
  ul{
    display: flex;
    justify-content: end;
    color: black;
    list-style: none;
    text-align: center;
    padding-top: 1vh;
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
  button:hover a{
    color: white;
  }
    

</style>
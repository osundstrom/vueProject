<script>
import navigation from '../components/navigation.vue'
import oneItem from '../components/oneItem.vue'

let url = "http://localhost:3000/item";

export default {
    data(){
    return {
      allItems:[]
      
    }
  },
    components: {navigation,oneItem},
    
    methods: {
        async getItems() {
            try {
                const response = await fetch(url);
                const data = await response.json();
                console.log(data);
                this.allItems = data;
                
            } catch (error) {
                console.error(error)
            }
        }
        },
        mounted() {
            this.getItems();
    }
    
  }

</script>

<template>

<main>
    <navigation/>
<h1 class="d-flex justify-content-center">Lagersaldo</h1>
    <div id="divItems" class="container " >   
        <div class="row d-flex justify-content-start">
            
        <oneItem v-for="oneItem in allItems" :key="oneItem._id" :oneItem="oneItem" />
    
    </div>
    </div>
    
</main>
</template>

<style scoped>
main{
    background-image: url("/src/assets/backMain.jpg");
    background-size: cover; 
    background-position: center; 
    min-height: 100vh;
    color: white;
    
}


#divitems{
    margin-top: 2rem;
}


</style>


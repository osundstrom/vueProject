<script>
import navigation from '../components/navigation.vue'
import oneItem from '../components/oneItem.vue'
import addItem from '../components/addItem.vue'
import editItem from "../components/editItem.vue";

let url = "http://localhost:3000/item";

export default {
    data(){
    return {
      allItems:[],
      
      editingItem: null,
    }
  },
    components: {navigation,oneItem,addItem,editItem},
    
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
        },

        async deleteItem(_id) { //funktion radera baserat på _id
        try{
          const res = await fetch(url + "/"+  _id, { //radera baserat på id
            method: "DELETE", //radera
            headers: {
              "Accept": "application/json", //accepterar json
              "Content-type": "application/json" //skickar json
             }
          });
          const data = await res.json(); 

          this.getItems(); 
          
        } catch(error) { //vid error
          console.error(error)
        }
      },


      openEdit(item) {
      this.editingItem = {...item}; 
    },

    closeEdit() {
      this.editingItem = null; 
    },

    editedItemUpdated(updatedItem) {

      const index = this.allItems.findIndex((item) => item._id === updatedItem._id);
      if (index !== -1) {
        this.allItems[index] = updatedItem;
      }
      this.closeEdit(); 
    },


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
<addItem @itemAdded="getItems"/>
    <div id="divItems" class="container " >   
        <div class="row d-flex justify-content-start">
            
        <oneItem v-for="oneItem in allItems" :key="oneItem._id" :oneItem="oneItem" @deleteItem="deleteItem" @editItem="openEdit" />
    
    </div>
    </div>

    <editItem
      v-if="editingItem"
      :itemEdited="editingItem"
      @updatedEdit="editedItemUpdated"
      @closedEdit="closeEdit"
       @getItems="getItems"
    />

    
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


<script>
import navigation from '../components/navigation.vue'
import oneItem from '../components/oneItem.vue'
import addItem from '../components/addItem.vue'
import editItem from "../components/editItem.vue";

let url = "https://projektramverkbackend.onrender.com/item";

export default {
    data(){
    return {
      allItems:[],
      searchText:"",
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

        computed: {
         searchedItem() {


            if(this.searchText === "") {
                console.log("helt tom")
                return this.allItems;
            }else {
                  
                return this.allItems.filter((item) => {
                    const brandSearch = item.itemBrand && item.itemBrand.toLowerCase().includes(this.searchText.toLowerCase());
                    const codeSearch = item.itemCode && item.itemCode.toLowerCase().includes(this.searchText.toLowerCase());
                    return brandSearch || codeSearch;
                });

            }
        }
    },

        mounted() {
            console.log("Mounted: searchText is", this.searchText);
            this.getItems();
            
    }
    
  }

</script>

<template>

<main>
    <navigation/>
    <h1 class="d-flex justify-content-center">Lagersaldo</h1>
<div class="container">
<button class="btn btn-dark" type="button" data-bs-toggle="offcanvas" data-bs-target="#offcanvasTop" aria-controls="offcanvasTop"><i class="fa-solid fa-plus"></i> Ny produkt</button>

<div class="offcanvas offcanvas-top" tabindex="-1" id="offcanvasTop" aria-labelledby="offcanvasTopLabel">
  <div class="offcanvas-header">
    
    <button type="button" class="btn-close" data-bs-dismiss="offcanvas" aria-label="Close"></button>
  </div>
  <div class="offcanvas-body">
    <addItem @itemAdded="getItems"/>
  </div>
</div>
</div>

    <div id="divItems" class="container " >   
        <div class="row d-flex justify-content-start">

        <div class="container">
    <div id="searchDiv" class="">
      <input type="text" v-model="searchText" placeholder="Sök produkt" class="form-control"/>
    </div>
    </div>
    <oneItem v-for ="oneItem in searchedItem" :key="oneItem._id" :oneItem="oneItem" @deleteItem="deleteItem" @editItem="openEdit"/>
    
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
    background: 
      linear-gradient(rgba(255, 255, 255, 0.1), rgba(0, 0, 0, 0.2)),
      url("/src/assets/backMain.jpg");
    background-size: cover; 
    background-position: center; 
    min-height: 100vh;
    color: white;
    
}


#divitems{
    margin-top: 2rem;
    
}

#searchDiv{
    margin-top: 2vh;
}

.offcanvas{
  background-color: aliceblue;
  top: 0;
  overflow-x: hidden ;
  height: fit-content;
}

h1{
  text-shadow: -1px -1px 0 #000, 1px -1px 0 #000, -1px 1px 0 #000, 1px 1px 0 #000;
}



</style>
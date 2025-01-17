<script>
//importerar
import navigation from '../components/navigation.vue'
import oneItem from '../components/oneItem.vue'
import addItem from '../components/addItem.vue'
import editItem from "../components/editItem.vue";

//URL
let url = "https://projektramverkbackend.onrender.com/item";

export default {
  //data för komponent
    data(){
    return {
      allItems:[], //tom array för alla produkter
      searchText:"", //tomt sökfält
      editingItem: null, //produkt som redigeras
    }
  },

  //komponeter som anväönds
    components: {navigation,oneItem,addItem,editItem},
    
    methods: {
      //funktin för hämta alla produkter
        async getItems() {
          //kollar efter token
          const token = localStorage.getItem("token");
          
          //om inge token
          if (!token) {
            console.error("Ingen token hittas");
            return;
        }
          //skickar en fetch förfrågan för att hmta från URL.
            try {
                
                const response = await fetch(url, {
                  method: "GET",
                  headers: {
                    "Authorization": `Bearer ${token}`,
                    "Content-Type": 'application/json'
                  }
                });
                const data = await response.json(); //väntar svar och till JSON
                console.log(data);
                this.allItems = data; //sparar alla i allItems
                
            } catch (error) { //vid error
                console.error(error)
            }
        },

        async deleteItem(_id) { //funktion radera baserat på _id
          const token = localStorage.getItem("token");//hämtar token

          if (!token) { //om token ej finns
          console.error("Ingen token hittades");
          return;
    }
        try{
          const res = await fetch(url + "/"+  _id, { //radera baserat på id
            method: "DELETE", //radera
            headers: {
              "Accept": "application/json", //accepterar json
              "Content-type": "application/json", //skickar json
              "Authorization": `Bearer ${token}`
             }
          });
          const data = await res.json(); 

          this.getItems(); //hämtar
          
        } catch(error) { //vid error
          console.error(error)
        }
      },

      //öppnar formulär för redigering
     openEdit(item) {
      this.editingItem = {...item}; 
    },

    //Stänger formulär för redigering
     closeEdit() {
      this.editingItem = null; 
    },

    //uppdaterar produkten som redigerats
    editedItemUpdated(updatedItem) {

      const index = this.allItems.findIndex((item) => item._id === updatedItem._id);
      if (index !== -1) {
        this.allItems[index] = updatedItem; //uppdaterar i listan
      }
      this.closeEdit(); //stäng
    },

    

        },
        //en cumputed för att kunna söka via sökrutan
        computed: {
          //sökning
         searchedItem() {
              //om sökrutan är tom får vi allt
            if(this.searchText === "") {
                console.log("helt tom")
                return this.allItems;
            }else {
                  //annars får vi endast de som stämmer överens baserat på märke och produktnummer (itemBrand, itemCode)
                return this.allItems.filter((item) => {
                    const brandSearch = item.itemBrand && item.itemBrand.toLowerCase().includes(this.searchText.toLowerCase());
                    const codeSearch = item.itemCode && item.itemCode.toLowerCase().includes(this.searchText.toLowerCase());
                    return brandSearch || codeSearch;
                });

            }
        }
    },
      //körs då sidan laddats in.
        mounted() {
            console.log("Mounted: searchText is", this.searchText);
            this.getItems(); //hämtar all produkter
            
    }
    
  }

</script>

<template>

<main>
  <!--Navigering-->
    <navigation/>
    <h1 class="d-flex justify-content-center">Lagersaldo</h1>
<div class="container">
  <!--Öppna formulär-->
<button class="btn btn-dark" type="button" data-bs-toggle="offcanvas" data-bs-target="#offcanvasTop" aria-controls="offcanvasTop"><i class="fa-solid fa-plus"></i> Ny produkt</button>

<div class="offcanvas offcanvas-top" tabindex="-1" id="offcanvasTop" aria-labelledby="offcanvasTopLabel">
  <div class="offcanvas-header">
    <!--stäng formulär-->
    <button type="button" class="btn-close" data-bs-dismiss="offcanvas" aria-label="Close"></button>
  </div>
  <div class="offcanvas-body">
    <!-- Lägg till produkt formuläret-->
    <addItem @itemAdded="getItems"/>
  </div>
</div>
</div>

    <div id="divItems" class="container " >   
        <div class="row d-flex justify-content-start">
  <!--Sökruta så man kan filtrera-->
        <div class="container">
    <div id="searchDiv" class="">
      <input type="text" v-model="searchText" placeholder="Sök produkt" class="form-control"/>
    </div>
    </div>
    <!--Loopa igenom och visa respektive produkt i en oenItem-->
    <oneItem v-for ="oneItem in searchedItem" :key="oneItem._id" :oneItem="oneItem" @deleteItem="deleteItem" @editItem="openEdit"/>
    
    </div>
    </div>

    <!--Redigeringsformulär, visas endast om editingItem som definerades ovan inte är null.-->
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
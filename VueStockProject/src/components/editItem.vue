<script>

export default {
  props: { //te emot data från stockView
    itemEdited: {
      type: Object, //typ ska vara ett objekt
      default: null, //standardvärde 
    },
    
  },
  data() {
    return{
        errorMessage: "", //felmeddelanden
    }
  },
  methods: {
    //funktion uppdatera produkt
    async updateItem() {
      const token = localStorage.getItem("token");

      if (!token) {
        console.error("Ingen token finns");
        return;
      }

      try {
        const updatedItem = this.itemEdited;
        //PUT förfrågan 
        const response = await fetch(`https://projektramverkbackend.onrender.com/item/${updatedItem._id}`, {
          method: "PUT",
          headers: {
            "Accept": "application/json",
            "Content-type": "application/json",
            "Authorization": `Bearer ${token}`,
          },
          body: JSON.stringify(updatedItem),//Skcikar uppdaterade
        });

          const data = await response.json(); //väntar svar och till JSON

          //skickar vie emit 
          this.$emit("updatedEdit", data); //uppdaterar produkt 
          this.$emit("getItems"); //hämtar produkter
        
      } catch (error) {
        console.error(error);
      }
    },

    //avbryte redigering
    closeEdit() {
      this.$emit("closedEdit"); 
    },

    //funktion ladda upp bild
    uploadFile(x) {
      const file  = x.target.files[0]; //Hämtar valda filen
      const maxSize = 500 * 1024; //filstorlek
      if (file) {
        if(file.size > maxSize) { //om för stor fil
            this.errorMessage = "Fil är för stor, max 500kb";
            this.item.itemImage = null;
            x.target.value = "";
            return;
        }else {
            this.errorMessage = ""; 
        }
        //lägg till check för filtyp.

        if (!file.type.startsWith("image/")) { //om de ej är en bild
            this.errorMessage = "Endast bildfiler är tillåtna";
            this.item.itemImage = null;
            x.target.value = null;
            return;
        }else {
          this.errorMessage = "";
        }
        //läsa in bilden med fielereader
        const reader = new FileReader();
        reader.readAsDataURL(file); 
        reader.onload = () => {
          this.itemEdited.itemImage = reader.result; //sätter DataUrl som itemImage så den uppdateras på hemsidan
        };
        
      }
    },
  },
};
     
     
</script>


<template>
  <div v-if="itemEdited" class="container">
    <!--Formulär för att redigera en produkt-->
    <form @submit.prevent="updateItem"> <!--Förhinda formulrs vanliga och vue lyssnar submit-->
  <div class="row g-3">
  <div class="col">
    <label for="itemCode" class="form-label">Produktnummer</label>
     <!-- v-model för värdet  item.itemCode -->
    <input id="itemCode" type="text" v-model="itemEdited.itemCode" class="form-control" placeholder="Artikelnummer" aria-label="item code"  required>
  </div>
  <div class="col">
    <label for="itemBrand">Märke</label>
     <!-- v-model för värdet  item.itemBrand -->
    <input id="itemBrand" type="text" v-model="itemEdited.itemBrand" class="form-control" placeholder="Märke" aria-label="item brand"  required>
  </div>
  <div class="col">
    <label for="ItemStock">Saldo</label>
     <!-- v-model för värdet  item.itemStock -->
    <input id="itemStock" type="number" v-model="itemEdited.itemStock" class="form-control" placeholder="Antal i lager" aria-label="item stock"  required>
  </div>
  <div class="col">
    <label for="itemPrice">Pris</label>
     <!-- v-model för värdet  item.itemPrice -->
    <input id="itemPrice" type="number" v-model="itemEdited.itemPrice" class="form-control" placeholder="Pris" aria-label="item price"  required>
  </div>
  <div class="mb-3">
  <label for="formFile" class="form-label">Lägg till bild</label>
   <!-- Lägga till bild -->
  <div id="errorDiv" v-if="errorMessage" class="alert alert-primary" >{{ errorMessage }}</div>
  <input class="form-control" @change="uploadFile"  type="file" id="formFile" >
</div>

<div class="col">
    <button class="btn btn-primary" type="submit">Uppdatera</button>
     <!-- vid klick stänger formuläret-->
    <button type="button" @click="closeEdit" class="btn btn-secondary">Cancel</button>
  </div>
</div>
</form>
</div>
</template>


<style scoped>
.container {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  color: black;
}

form {
  background-color: white;
  padding: 20px;
  border-radius: 5px;
  width: 50%;
}

</style>

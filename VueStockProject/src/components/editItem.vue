<script>

export default {
  props: {
    itemEdited: {
      type: Object,
      default: null,
    },
    
  },
  data() {
    return{
        errorMessage: "",
    }
  },
  methods: {
    async updateItem() {
      try {
        const updatedItem = this.itemEdited;
        const response = await fetch(`http://localhost:3000/item/${updatedItem._id}`, {
          method: "PUT",
          headers: {
            "Accept": "application/json",
            "Content-type": "application/json",
          },
          body: JSON.stringify(updatedItem),
        });

          const data = await response.json();
          this.$emit("updatedEdit", data); 
          this.$emit("getItems"); 
        
      } catch (error) {
        console.error(error);
      }
    },

    closeEdit() {
      this.$emit("closedEdit"); 
    },

    uploadFile(x) {
      const file  = x.target.files[0];
      const maxSize = 500 * 1024;
      if (file) {
        if(file.size > maxSize) {
            this.errorMessage = "Fil är för stor, max 500kb";
            this.item.itemImage = null;
            x.target.value = "";
            return;
        }else {
            this.errorMessage = "";
        }
        //lägg till check för filtyp.

        if (!file.type.startsWith("image/")) {
            this.errorMessage = "Endast bildfiler är tillåtna";
            this.item.itemImage = null;
            x.target.value = null;
            return;
        }else {
          this.errorMessage = "";
        }
        
        const reader = new FileReader();
        reader.onload = () => {
          this.itemEdited.itemImage = reader.result; 
        };
        reader.readAsDataURL(file); 
      }
    },
  },
};
     
     
</script>


<template>
  <div v-if="itemEdited" class="container">
    <form @submit.prevent="updateItem">
  <div class="row g-3">
  <div class="col">
    <label for="itemCode" class="form-label">Produktnummer</label>
    <input id="itemCode" type="text" v-model="itemEdited.itemCode" class="form-control" placeholder="Artikelnummer" aria-label="item code"  required>
  </div>
  <div class="col">
    <label for="itemBrand">Märke</label>
    <input id="itemBrand" type="text" v-model="itemEdited.itemBrand" class="form-control" placeholder="Märke" aria-label="item brand"  required>
  </div>
  <div class="col">
    <label for="ItemStock">Saldo</label>
    <input id="itemStock" type="number" v-model="itemEdited.itemStock" class="form-control" placeholder="Antal i lager" aria-label="item stock"  required>
  </div>
  <div class="col">
    <label for="itemPrice">Pris</label>
    <input id="itemPrice" type="number" v-model="itemEdited.itemPrice" class="form-control" placeholder="Pris" aria-label="item price"  required>
  </div>
  <div class="mb-3">
  <label for="formFile" class="form-label">Lägg till bild</label>
  <div id="errorDiv" v-if="errorMessage" class="alert alert-primary" >{{ errorMessage }}</div>
  <input class="form-control" @change="uploadFile"  type="file" id="formFile" >
</div>

<div class="col">
    <button class="btn btn-primary" type="submit">Uppdatera</button>
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

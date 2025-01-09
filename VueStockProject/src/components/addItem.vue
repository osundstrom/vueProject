<script>
import OneItem from './oneItem.vue';


    //exports
    export default {
        name: "addItem",
    data() {
        return { 
            item: { 
                itemCode: "", 
                itemBrand: "", 
                itemStock: "", 
                itemPrice: "",
                itemImage: "",
            },
            errorMessage: "",
            
                
            
        };
    },
    emits:["itemAdded"], 
    methods: {
        async uploadFile(x){
            let file = x.target.files[0];
            const maxSize = 500 * 1024;
            if(file) {
                if(file.size > maxSize) {
                    this.errorMessage = "Fil är för stor, max 500kb";
                    this.item.itemImage = null;
                    x.target.value = "";
                    return;
                }else{
                    this.errorMessage = "";
                }

                if (!file.type.startsWith("image/")) {
                    this.errorMessage = "Endast bildfiler är tillåtna";
                    this.item.itemImage = null;
                    x.target.value = "";
                    return;
                 }else {
                    this.errorMessage = "";
                }
                try{
                 
                const reader = new FileReader(); 
                reader.readAsDataURL(file); 
                reader.onload = () => {
                    this.item.itemImage = reader.result; 
                };
                }catch (error){
                    console.error(error)
                }
            }
        },
        
        async addItem(){ 
            try {
                
                    let fullItem = this.item
                    
                    const response = await fetch("http://localhost:3000/item", {
                        method:"POST", 
                        headers: {
                                "Accept": "application/json", 
                                "Content-type": "application/json" 
                                },
                        body: JSON.stringify(fullItem) 
                        })

                    const data = await response.json();
                   
                    this.item.itemCode = "";
                    this.item.itemBrand = "";
                    this.item.itemStock = 0;
                    this.item.itemPrice = 0;
                    this.item.itemImage = "";

                    document.getElementById("formFile").value = "";
                    
                    
                    this.$emit("itemAdded")            
                    }
            
            catch(error) { 
                console.error(error)
            }}
        
        }
};
</script>


<template>
  <div class="container">
    <form @submit.prevent="addItem">
  <div class="row g-3">
  <div class="col">
    <label for="itemCode">Artikelnummer</label>
    <input type="text" class="form-control" placeholder="Artikelnummer" aria-label="item code" v-model="item.itemCode" required>
  </div>
  <div class="col">
    <label for="itemBrand">Märke</label>
    <input type="text" class="form-control" placeholder="Märke" aria-label="item brand" v-model="item.itemBrand" required>
  </div>
  <div class="col">
    <label for="itemStock">Saldo</label>
    <input type="number" class="form-control" placeholder="Antal i lager" aria-label="item stock" v-model="item.itemStock" required>
  </div>
  <div class="col">
    <label for="itemPrice">Pris</label>
    <input type="number" class="form-control" placeholder="Pris" aria-label="item price" v-model="item.itemPrice" required>
  </div>
  
  <div class="mb-3">
  <label for="formFile" class="form-label">Lägg till bild</label>
  <div id="errorDiv" v-if="errorMessage" class="alert alert-primary" >{{ errorMessage }}</div>
  <input class="form-control" type="file" id="formFile" @change="uploadFile" >
</div>

<div class="col">
    <button class="btn btn-dark" type="submit">Lägg till</button>
  </div>
</div>
</form>
</div>
</template>


<style scoped>
#errorDiv{
    color: black;
}




</style>

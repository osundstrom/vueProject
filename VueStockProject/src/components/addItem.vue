<script>
import OneItem from './oneItem.vue';


    //exports
    export default {
        name: "addItem",
    data() {
        return { 
            item: { //item med tomt på tänkt inehåll från formulär
                itemCode: "", 
                itemBrand: "", 
                itemStock: "", 
                itemPrice: "",
                itemImage: "",
            },
            errorMessage: "", //felmeddelande
            
                
            
        };
    },
    ////emit till stockView
    emits:["itemAdded"], 
    methods: {
        //När en bild laddas upp
        async uploadFile(x){
            let file = x.target.files[0]; //hämtar valda filen, (position [0])
            const maxSize = 500 * 1024; //sätter max storlek (500kb/0,5mb)
            if(file) {
                if(file.size > maxSize) { //om bild är för stor
                    this.errorMessage = "Fil är för stor, max 500kb";
                    this.item.itemImage = null;
                    x.target.value = "";
                    return;
                }else{
                    this.errorMessage = "";
                }

                if (!file.type.startsWith("image/")) { //om det ej är en bild
                    this.errorMessage = "Endast bildfiler är tillåtna";
                    this.item.itemImage = null;
                    x.target.value = "";
                    return;
                 }else {
                    this.errorMessage = "";
                }
                try{
                 
                //läsa in filen med fielereader, läser den som en DataUrl
                const reader = new FileReader(); 
                reader.readAsDataURL(file); 
                reader.onload = () => {
                    this.item.itemImage = reader.result; //sätter DataUrl som itemImage så den uppdateras på hemsidan
                };
                }catch (error){
                    console.error(error)
                }
            }
        },
        
        async addItem(){ 
            try {

                    let fullItem = this.item //hämtar item

                    const token = sessionStorage.getItem("token"); //hämtar token
                    if (!token) { //om ej finns
                        console.error("Ingen token hittas");
                        return;
                    }
                    
                    //POST förfrågan till backend
                    const response = await fetch("https://projektramverkbackend.onrender.com/item", {
                        method:"POST", 
                        headers: {
                                "Accept": "application/json",  //response JSON
                                "Content-type": "application/json", //sätt till json
                                "Authorization": `Bearer ${token}` //skickar med token 
                                },
                        body: JSON.stringify(fullItem) //till JSON
                        })
                        
                    //väntar på svar och JSON format. 
                    const data = await response.json();
                   
                    //töm formuläret
                    this.item.itemCode = "";
                    this.item.itemBrand = "";
                    this.item.itemStock = 0;
                    this.item.itemPrice = 0;
                    this.item.itemImage = "";

                    document.getElementById("formFile").value = "";
                    
                    //emit till stockView
                    this.$emit("itemAdded")            
                    }
            
            catch(error) { 
                console.error(error)
            }}
        
        }
};
</script>


<template>
    <!--Formulär för att lägga till ny prdukt, detta med bootstrap grid system-->
  <div class="container">
    <form @submit.prevent="addItem"> <!--Förhinda formulrs vanliga och vue lyssnar submit-->
  <div class="row g-3">
  <div class="col">
    <label for="itemCode">Artikelnummer</label>
    <!-- v-model för värdet  item.itemCode -->
    <input type="text" class="form-control" placeholder="Artikelnummer" aria-label="item code" v-model="item.itemCode" required>
  </div>
  <div class="col">
    <label for="itemBrand">Märke</label>
    <!-- v-model för värdet  item.itemBrand -->
    <input type="text" class="form-control" placeholder="Märke" aria-label="item brand" v-model="item.itemBrand" required>
  </div>
  <div class="col">
    <label for="itemStock">Saldo</label>
    <!-- v-model för värdet  item.itemStock -->
    <input type="number" class="form-control" placeholder="Antal i lager" aria-label="item stock" v-model="item.itemStock" required>
  </div>
  <div class="col">
    <label for="itemPrice">Pris</label>
    <!-- v-model för värdet  item.itemPrice -->
    <input type="number" class="form-control" placeholder="Pris" aria-label="item price" v-model="item.itemPrice" required>
  </div>
  
  <div class="mb-3">
    <!--Ladda upp bild, med error meddelanden-->
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

<!--scopade styles-->
<style scoped>
#errorDiv{
    color: black;
}




</style>

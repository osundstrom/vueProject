<script>
    export default {
        name: "oneItem",
        props:  {
            oneItem: Object
        },
        
    }
    

</script>

<template>
<div id ="cardDiv" class="col-lg-3 col-md-4 col-sm-6 col-12 mb-4 d-flex justify-content-center flex-wrap ">
<div class="card " style="width: 18rem;">
    <div id="imageDiv">
  <img v-bind:src="oneItem.itemImage" class="card-img-top" id="squareImage" alt="picture of product">
</div>
  <div class="card-body">
    <h5 class="card-title">{{oneItem.itemCode}} </h5>
    <p class="card-text"> Märke: {{oneItem.itemBrand}} </p>
    <p class="card-text"> Saldo:{{oneItem.itemStock}} </p>
    <p class="card-text"> Pris: {{oneItem.itemPrice}} </p>
    <button id="but1" type="button" @click="$emit(`deleteItem`, oneItem._id)" class="btn btn-danger"><i class="fa-regular fa-trash-can fa-sm"></i></button>
    <button id="but2" type="button" @click="$emit(`editItem`, oneItem)"class="btn btn-warning"><i class="fa-regular fa-pen-to-square fa-sm"></i></button>
  </div>
</div>
</div>

 

</template>

<style scoped>


#squareImage {
position: absolute;
top: 0;
left: 0;
width: 100%;
height: 100%;
object-fit: cover;
}

#imageDiv{
position: relative;
width: 100%;
padding-top: 100%;    
overflow: hidden;
}

.card{
  background-color: rgba(255, 255, 255, 0.70);
}

.card-body{
    text-align: center;
    
}

#cardDiv {
    padding: 1vw; 
    
}

#but1{
  float: left;
}
#but2{
  float: right;
}

</style>

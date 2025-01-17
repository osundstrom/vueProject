
<script>

//URL
let url = "https://projektramverkbackend.onrender.com/login";
export default {
    name: "loginForm",

    
methods: {

    
//funktion för inlogg
    async loginFunc() {
        //console.log("test")

        //hämtar email och lösenord rån formulär
        const email = document.getElementById("InputEmail").value;
        const password = document.getElementById("InputPassword").value;
        let errorDiv = document.getElementById("errorDiv"); //ferlmeddelanden
        try {
            //POST förfrågan
            const response = await fetch(url, {
                method: "POST",
                headers: { "Content-Type": "application/json",
            },

            //Skickar mail och lösen
            body: JSON.stringify({
                email: email,
                password: password
            })
        }) 

        //om misslyckas
        if (!response.ok) { 
        console.log("error")
        errorDiv.textContent = "Felaktigt användarnamn/lösenord"; 
        throw new Error("Felaktigt användarnamn/lösenord"); 
        
      }
        
      //Väntar svar och till JSON
        const data = await response.json();
        console.log(data);
        console.log("token:", data.recivedToken.token);
        //Sparar token i loclastorage
        sessionStorage.setItem("token", data.recivedToken.token);

        //Om token finns så skcikas man vidare till /stock 
        if (data.recivedToken && data.recivedToken.token) {
        this.$router.push("/stock");
        }else {
            throw new Error("kunde ej ändra till stock sidan");
        }

        }catch (error) {
            console.error(error);
            errorDiv.textContent = "Felaktigt användarnamn/lösenord";   
        }
    }

}
}
</script>




<template>
    
<div id="fullform" class="conatiner-md" >
    

<form @submit.prevent="loginFunc" class="container-md">
    <!-- Formulär inloggning -->
  <div class="mb-3">
    <label for="InputEmail" class="form-label">Användare</label>
    <input type="email" class="form-control" id="InputEmail" aria-describedby="emailHelp" placeholder="name@example.com">
  </div>
        <div class="mb-3">
            <label for="InputPassword" class="form-label">Lösenord</label>
            <input type="password" class="form-control" id="InputPassword" placeholder="Lösenord">
        </div>
        
            <button type="submit" class="btn btn-primary">Logga in</button>
</form>

</div>
<div id="errorDiv"></div>

</template>




<style scoped>
 
    form{
        background-color: rgba(255, 255, 255, 0.75);
        padding: 2%;
        border-radius: 10px;

        @media (min-width: 750px) {
        width: 40vw;
    }
    }
  

  #fullform{
    color: white;
    text-shadow: -1px -1px 0 #000, 1px -1px 0 #000, -1px 1px 0 #000, 1px 1px 0 #000;
    
}



  #errorDiv{
    display: flex;
    justify-content: center;
    color: white;
    text-shadow: -1px -1px 0 #ff0000, 1px -1px 0 #ff0000, -1px 1px 0 #ff0000, 1px 1px 0 #ff0000;
  }
    

</style>
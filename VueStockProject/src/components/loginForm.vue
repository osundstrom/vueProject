
<script>

let url = "https://projektramverkbackend.onrender.com/login";
export default {
    name: "loginForm",

    
methods: {

    

    async loginFunc() {
        console.log("test")
        const email = document.getElementById("InputEmail").value;
        const password = document.getElementById("InputPassword").value;
        let errorDiv = document.getElementById("errorDiv");
        try {

            const response = await fetch(url, {
                method: "POST",
                headers: { "Content-Type": "application/json",
            },

            body: JSON.stringify({
                email: email,
                password: password
            })
        }) 

        if (!response.ok) { 
        console.log("error")
        throw new Error("inloggning misslyckades"); 
      }
        
        const data = await response.json();
        localStorage.setItem("token", data.recivedToken.token);

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
<div id="fullform" class="container-md">
    <div id="errorDiv"></div>

<form @submit.prevent="loginFunc">
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


</template>




<style scoped>
 
    form{
        width: 30vw;
        background-color: rgba(255, 255, 255, 0.75);
        padding: 2%;
        border-radius: 10px;
    }
  

  #fullform{
    display: flex;
    align-items: center;
    justify-content: center;
    color: white;
    text-shadow: -1px -1px 0 #000, 1px -1px 0 #000, -1px 1px 0 #000, 1px 1px 0 #000;
    
  }
    

</style>
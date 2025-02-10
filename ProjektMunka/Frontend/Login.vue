<template>
    <div class="register-container">
        <form class="register-form"  @submit.prevent="submitdata">
            <h2>Belépés</h2>
            <div id="back">
                <button id="button" type="button" class="btn-close" @click="goToBack" aria-label="Close"></button>
            </div>
            <div class="input-container">
                <input type="email" placeholder="Email..." v-model.trim="email" required>
                <i class="fa-solid fa-envelope"></i>
            </div>
            <p v-if=" emailvalidate" style="color: red">{{ emailvalidate }}</p>
            <div class="input-container">
                <input type="Password" placeholder="Jelszó..." v-model.trim="password" required>
                <i class="fa-solid fa-lock"></i>
            </div>
            <p v-if=" passwordvalidate" style="color: red">{{ passwordvalidate }}</p>
            <p v-if="!submitted" style="color: red">Hibás email vagy jelszó</p>          
            <button type="submit">Bejelentkezés</button>
        </form>
  
    </div>
</template>


<script>
export default {
    data(){
            return{
                email:"",
                password:"",
                submitted:true
            };
        },
        computed:{
            emailvalidate(){
            if(this.email==="" && this.submitted===false){
                return false;
            }
            if(this.email===""){
                return "Ne hagyd üresen a mezőt";
            }
           
            const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
            return emailRegex.test(this.email) ? "":"Érvénytelen email cím";
        },

        phonevalidate(){
            if(this.phone===""){
                return "Ne hagyd üresen a mezőt";
            }
            const phoneregex=/^(?:\+36|06)\s?(?:30|20|70)\s?\d{3}\s?\d{4}$/;
            return phoneregex.test(this.phone) ? "": "Érvénytelen telefonszám!";
        },


        passwordvalidate(){
            if(this.password==="" && this.submitted===false){
                return false;
            }
            if(this.password===""){
                return "Ne hagyd üresen a mezőt";
            }
            else if(this.password.length<=8){
                return "A jelszónak legalább 8 karakter hosszúnak kell lennie";
            }
          
        },
        },
        methods:{
            async submitdata(){
                const success = await this.$store.dispatch('user/loginUser', {
                    email: this.email,
                    password: this.password
                    
                });
                
                if (!success) {
                    this.submitted=false;
                    return;
                 
                
                }
                else{
                    console.log("sikeres belépés",this.$store.state.user);
                    this.submitted=true;
                    return this.$router.push('/home');
                }
            },
          
            goToBack(){
                return this.$router.push('/home');
            },
        
            
        },
    
}

</script>


<style scoped>
.input-container {
  position: relative;
  margin: 10px 0;
}

.input-container i {
  position: absolute;
  right: 10px;
  top: 50%;
  transform: translateY(-50%);
}

#button{
    background-color: transparent;
    border: none;
    color: black;
}
#back{
    position: absolute;
    top: -25px;
    right: 20px;
    background-color: transparent;
    border: none;
    color: black;
}

html, body {
  margin: 0;
  padding: 0;
  height: 100%;
  width: 100%;
  box-sizing: border-box;
}

body {
  display: flex;
  justify-content: center;
  align-items: center;
}

.register-container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  width: 100vw;
  background-image: url('https://learn.humsa.com/wp-content/uploads/2023/11/image-188.jpeg');
  background-size: cover;
  background-position: center;
  font-family: 'Roboto', sans-serif;
  background-repeat: no-repeat;
}

.register-form {
   
  background: rgba(255, 255, 255, 0.8);
  backdrop-filter: blur(8px);
  background: transparent;
    padding: 20px;
  border-radius: 15px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.5);
    text-align: center;
    width: 50%; 
    max-width: 500px; 
}


button {
    margin-top:20px ;
  background-color: rgb(240, 169, 63);
  border: 1px solid rgb(240, 169, 63);
  border-radius: 5px;
  width: 100%;
  height: 40px;
  font-size: 1.2rem;
}

input{
    border-top-color: transparent;
    border-right-color: transparent;
    border-left-color: transparent;
    border-bottom-color:rgb(240, 169, 63) ;
    border-radius: 5px;
    padding: 10px;
    width: 100%;
    margin: 5px;
    background: transparent;
}
input::placeholder{
    color: black;
}

button:hover {
    background-color: rgba(240, 169, 63, 0.8);
    transition: all 0.3s ease;
   
}


</style>
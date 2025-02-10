<template>
    <div class="register-container">
        <form class="register-form" @submit.prevent="submitdata">
            <h2>Regisztráció</h2>
            <div  id="back">
                <button id="button" type="button" class="btn-close" @click="goToBack" aria-label="Close"></button>
            </div>

            <div class="input-container">
                <input type="text" placeholder="Felhasználónév..." v-model.trim="userdata.Username" required>
                <i class="fa-solid fa-user"></i>
            </div>
            <p v-if="usernamevalidate" style="color: red">{{ usernamevalidate }}</p>

            <div class="input-container">
                <input type="email" placeholder="Email..." v-model.trim="userdata.email" required>
                <i class="fa-solid fa-envelope"></i>
            </div>
            <p v-if=" emailvalidate" style="color: red">{{ emailvalidate }}</p>
            

            <div class="input-container">
                <input type="Password" placeholder="Jelszó..." v-model.trim="userdata.password" required>
                <i class="fa-solid fa-lock"></i>
            </div>
            <p v-if=" submitted && passwordvalidate" style="color: red">{{ passwordvalidate }}</p>


            <div class="input-container">
                <input type="Password" placeholder="Jelszó újra..." v-model.trim="userdata.password_confirmation" required>
                <i class="fa-solid fa-lock"></i>
            </div>
            <p v-if=" submitted && passwordvalidate" style="color: red">{{ passwordvalidate }}</p>


            <div class="input-container">
                <input type="text" placeholder="Teljes név..." v-model.trim="userdata.FullName" required>
                <i class="fa-solid fa-user"></i>
            </div>
            <p v-if="fullNamevalidate" style="color: red">{{ fullNamevalidate }}</p>

            <div class="input-container">
                <input type="text" placeholder="Telefonszám..." v-model.trim="userdata.PhoneNumber" required>
                <i class="fa-solid fa-mobile"></i>
            </div>
            <p v-if="phonevalidate" style="color: red">{{ phonevalidate }}</p>
            <p v-if="!submitted" style="color: red">Hibás email vagy jelszó</p>    
            <button type="submit">Regisztráció</button>
        </form>
    </div>
</template>


<script>
export default {
        data(){
            return{
                userdata:{
                    Username:"",
                    email:"",
                    password:"",
                    password_confirmation:"",
                      FullName:"",
                    PhoneNumber:"",
                },
                submitted:true
            };
        },
        computed:{
            emailvalidate(){
            if(this.userdata.email==="" && this.submitted===false){
                return false;
            }
            const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
            return emailRegex.test(this.userdata.email) ? "":"Érvénytelen email cím";
        },

        phonevalidate(){
            const phoneregex=/^(?:\+36|06)\s?(?:30|20|70)\s?\d{3}\s?\d{4}$/;
            return phoneregex.test(this.userdata.phone) ? "": "Érvénytelen telefonszám!";
        },


        passwordvalidate(){
                if(this.userdata.password!==this.userdata.password_confirmation){
                if(this.userdata.password==="" && this.userdata.password_confirmation!=="" || this.userdata.password_confirmation==="" && this.userdata.password!==""){
                    return "";
                }
                return "A két jelszó nem egyezik meg";
            }

            else if(this.userdata.password.length<=8){
                return "A jelszónak legalább 8 karakter hosszúnak kell lennie";
            }
          
        },

        usernamevalidate() {
        const userRegex = /^[A-Za-z0-9_-]{3,16}$/;
        return userRegex.test(this.userdata.Username)
            ? ""
            : "A felhasználónév 3-16 karakter, betűk, számok, kötőjel és aláhúzás engedélyezett.";
    },

    fullNamevalidate() {
        if (this.userdata.FullName.trim() === "") {
            return "Ne hagyd üresen a mezőt.";
        }
        const nameRegex = /^[A-Za-zÁ-Úá-ú]+(\s[A-Za-zÁ-Úá-ú]+)+$/;
        return nameRegex.test(this.userdata.FullName)
            ? ""
            : "Adj meg legalább két szót, betűkkel és ékezetekkel.";
    },

        },
        methods:{
           async submitdata(){
                const success=await this.$store.dispatch('user/registerUser',{
                    Username:"",
                    email:"",
                    password:"",
                    password_confirmation:"",
                    FullName:"",
                    PhoneNumber:"",
                });
                
                if(!success){
                    console.log("sikertelen regisztráció");
                    this.submitted=false;
                    return;
                }
                else{
                    console.log("sikeres regisztráció",this.$store.state.user);
                    this.submitted=true;
                    this.$router.push('/home');
                }
            },
            goToBack(){
                return this.$router.push('/home');
            }
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
  backdrop-filter: blur(5px);
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
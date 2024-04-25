<script setup>
import axios from "axios";
import {ref} from "vue";
import { useRouter } from 'vue-router'; // Importa useRouter en lugar de router

const email = ref("");
const password = ref("");
const errMessage = ref("");
const URLimage = "https://img.freepik.com/free-vector/login-concept-illustration_114360-739.jpg";
const error = ref(false);   
const isWaiting = ref(false);
const router = useRouter(); // Obtiene el enrutador

const handleSubmit = async () => {
    try {
        isWaiting.value = false;
        error.value = false;
        isWaiting.value = true;
        const response = await axios.post("users/login", {
            email: email.value,
            password: password.value
        });
        if (response.status === 200) {
            console.log(response);
            localStorage.setItem("token", response.data.token);
            //router.push({ name: 'Home' });
        }
    } catch (err) {
        isWaiting.value = false;
        error.value = true;
        console.log("sapa");
        console.log(err);
        if (err.response.data.message === "Invalid password" || err.response.data.message === "Email does not exist") {
            errMessage.value = "Email or password incorrect";
        } else {
            errMessage.value = "We have problems :c";
        }
    }
};

const checkConnection = async () => {
    console.log("running request");
    const response = await axios.get("tours");
    console.log(response);
};
</script>

<template>
    <div class="LoginView-Container">
        <h3>
            Welcome!
        </h3>
        <form @submit.prevent="handleSubmit" class="mx-5">
            <div class="mb-3">
                <label for="email" class="form-label">Email address</label>
                <input type="email" class="form-control" id="email" v-model="email" placeholder="name@example.com"
                    required>
            </div>
            <div class="mb-3">
                <label for="password" class="form-label">Password</label>
                <input type="password" class="form-control" id="password" v-model="password" required>
            </div>
            <div class="col-auto">
                <button type="submit" class="btn btn-outline-primary" :disabled="isWaiting">  
                    <span role="status">Sign in</span>
                    <span v-if="isWaiting" class="spinner-border spinner-border-sm mx-1" aria-hidden="true"></span>
                </button>
            </div>
            <div v-if="error">
                <p class="text-danger text-center my-2">{{ errMessage }}</p>
            </div>
        </form>
        <div class="d-flex flex-column align-items-center">
            <a href="/signup" class="text-secondary rounded-1 text-start">Create an account</a>
            <a href="#" class="text-secondary rounded-1">Forgot your password?</a>
        </div>
    </div>
    <div class="Imagen-container">
        <img :src="URLimage" alt="Descripción de la imagen">
    </div>
</template>


<style lang="scss">
//@import './path.css'
$main-color: rgb(233, 233, 233);
$second-color: rgb(227, 227, 227);


.LoginView-Container {
    background-color: $main-color;
    display: flex;
    align-items: center;
    justify-content: space-evenly;
    flex-direction: column;
    height: 400px;
    border-radius: 8px;
    /*
    form{
        background-color: rgb(255, 255, 255);
        display:flex;
        flex-direction: column;
        width: 80%;
        border: none;
        label{
            padding: 10px;
        }
        input{
            height: 30px;
            border: none;
            background-color: $second-color;
        }
    }
    */
}

.Imagen-container{
    height: 400px;
    display: flex;
    align-items: center;
    justify-content: center;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1); /* offsetX offsetY blurRadius spreadRadius color */
    border-radius: 10px;
    margin-left: -16px;
    z-index: -1;
    //border: 2px solid rgb(150, 150, 150)
    user-select: none;
    img{
        pointer-events: none;
        /* Haz que la imagen ocupe el 100% de la altura del contenedor */
        height: 80%;
        /* Para que la imagen mantenga su aspecto original */
        width: auto;
    }

}
</style>

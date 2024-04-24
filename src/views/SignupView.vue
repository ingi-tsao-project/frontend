<script setup>
import axios from "axios";
import { ref } from "vue";
import { useRouter } from 'vue-router'; // Importa useRouter en lugar de router
const router = useRouter(); // Obtiene el enrutador

const name = ref("");
const email = ref("");
const password = ref("");
const passwordConfirm = ref("");

const errMessage = ref("");
const URLimage = "https://img.freepik.com/free-vector/login-concept-illustration_114360-739.jpg";
const error = ref(false);
const isWaiting = ref(false);

const handleSubmit = async () => {
    try {
        isWaiting.value = false;
        error.value = false;
        isWaiting.value = true;
        const response = await axios.post("users/signup", {
            name: name.value,
            email: email.value,
            password: password.value,
            passwordConfirm: passwordConfirm.value
        });
        if (response.status === 201) {
            router.push({ name: 'Home' });
        }
    } catch (err) {
        isWaiting.value = false;
        error.value = true;
        console.log(err);
        const res_message = err.response.data.message;
        if (res_message.includes("User validation failed: name")) {
            errMessage.value = "Name must be at least 4 characters";
        }
        else if (res_message.includes("passwordConfirm") && res_message.includes("same")) {
            errMessage.value = "Passwords are not the same";
        } else if (res_message.includes("duplicate key error collection")) {
            errMessage.value = "Email already exists";
        } else if (
            res_message.includes("password") && 
            res_message.includes("is shorter than the minimum allowed")
        ) { errMessage.value = "password must be at least 8 characters";
        } else { errMessage.value = "We have problems creating your account 😓"}
    }
};

</script>

<template>
    <div class="LoginView-Container">
        <h3>Welcome!</h3>
        <form @submit.prevent="handleSubmit" class="mx-auto w-75">
            <div class="mb-2">
                <label for="name" class="form-label">Name</label>
                <input type="name" class="form-control" id="name" v-model="name" required>
            </div>
            <div class="mb-2">
                <label for="email" class="form-label">Email</label>
                <input type="email" class="form-control" id="email" v-model="email" placeholder="name@example.com"
                    required>
            </div>
            <div class="row mb-2">
                <div class="col">
                    <label for="password" class="form-label">Password</label>
                    <input type="password" class="form-control" id="password" v-model="password" required>
                </div>
                <div class="col">
                    <label for="passwordConfirm" class="form-label">Confirm Password</label>
                    <input type="password" class="form-control" id="passwordConfirm" v-model="passwordConfirm" required>
                </div>
            </div>
            <div class="col-auto">
                <button type="submit" class="btn btn-outline-primary" :disabled="isWaiting">
                    <span role="status">Sign in</span>
                    <span v-if="isWaiting" class="spinner-border spinner-border-sm mx-1" aria-hidden="true"></span>
                </button>
            </div>
            <div v-if="error" class="text-danger text-center my-2 suavecito">
                {{ errMessage }}
            </div>
        </form>
        <div class="d-flex flex-column align-items-center mt-3">
            <a href="/signup" class="text-secondary">Create an account</a>
            <a href="#" class="text-secondary">Forgot your password?</a>
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
    transition: background-color 0.3s ease, font-size 0.3s ease;
    background-color: $main-color;
    display: flex;
    align-items: center;
    justify-content: space-evenly;
    flex-direction: column;
    padding: 20px 0px;
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

.Imagen-container {
    height: 400px;
    display: flex;
    align-items: center;
    justify-content: center;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    /* offsetX offsetY blurRadius spreadRadius color */
    border-radius: 10px;
    margin-left: -16px;
    z-index: -1;
    //border: 2px solid rgb(150, 150, 150)
    user-select: none;

    img {
        pointer-events: none;
        /* Haz que la imagen ocupe el 100% de la altura del contenedor */
        height: 80%;
        /* Para que la imagen mantenga su aspecto original */
        width: auto;
    }

}

.suavecito {
    transition: all 0.5s ease; /* Transición suave de opacidad */
}
</style>

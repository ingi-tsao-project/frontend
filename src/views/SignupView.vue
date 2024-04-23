<script>
import axios from "axios";
export default {
    name: "Signup",
    data() {
        return {
            email: "",
            password: "",
            errMessage: "",
            URLimage: "https://img.freepik.com/free-vector/login-concept-illustration_114360-739.jpg",
            error: false,
            isWaiting: false,
        };
    },
    methods: {
        async handleSubmit() {
            try {
                this.isWaiting = false;
                this.error = false;
                this.isWaiting = true;
                const response = await axios.post("users/login", {
                    email: this.email,
                    password: this.password
                });
                if (response.status === 200) {
                    this.$router.push({ name: 'home' });
                }
            } catch (err) {
                this.isWaiting = false;
                this.error = true;
                console.log(err.response.data.message);
                if (err.response.data.message = "Invalid password" || "Email does not exist"){
                    this.errMessage = "Email or password incorrect"
                }else {
                    this.errMessage = "We have problems :c"
                }


            }
        },
        async checkConnection() {
            console.log("running request")
            const response = await axios.get("tours");
            console.log(response);
        }
    }
}
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

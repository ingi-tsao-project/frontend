<script>
import axios from "axios";
export default {
    name: "Login",
    data() {
        return {
            email: "",
            password: "",
            error: false,
            isWaiting: false,
        };
    },
    methods: {
        async handleSubmit() {
            try {
                console.log("Runing request...");
                this.isWaiting = true;
                const response = await axios.post("users/login", {
                    email: this.email,
                    password: this.password
                });
                console.log(response.status)
                if (response.status === 200) {
                    this.$router.push({ name: 'home' });
                }
            } catch (err) {
                console.log(err.response.data.message);
                console.log(err.response);
                this.isWaiting = false;
                this.error = true;


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
        <h1>
            Welcome
        </h1>
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
                <p class="text-danger text-center my-2">Incorrect data :(</p>
            </div>
        </form>
        <div class="d-flex flex-column align-items-center">
            <a href="#" class="text-secondary rounded-1 text-start">Create an account</a>
            <a href="#" class="text-secondary rounded-1">Forgot your password?</a>
        </div>
    </div>

</template>

<style lang="scss">
//@import './path.css'
$main-color: rgb(213, 213, 213);
$second-color: rgb(227, 227, 227);


.LoginView-Container {
    background-color: $main-color;
    display: flex;
    align-items: center;
    justify-content: space-around;
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
</style>

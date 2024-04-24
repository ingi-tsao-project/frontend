<script setup>
import { ref } from 'vue';
import axios from "axios";
import Navbar from "@/components/Navbar.vue"
import TheWelcome from "@/components/TheWelcome.vue"


const email = ref("");
const password = ref("");
const errMessage = ref("");
const URLimage = "https://img.freepik.com/free-vector/login-concept-illustration_114360-739.jpg";
const error = ref(false);
const isWaiting = ref(false);

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
            $router.push({ name: 'home' });
        }
    } catch (err) {
        isWaiting.value = false;
        error.value = true;
        console.log(err.response.data.message);
        if (err.response.data.message === "Invalid password" || err.response.data.message === "Email does not exist"){
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
  <main>
    <Navbar class="sapa">

    </Navbar>
    <TheWelcome />
  </main>
</template>

<style lang="scss">
main{
  .sapa {
    position: fixed;
    top: 0;
    width: 100%;
    overflow: hidden;
    background-color: #333; /* Color de fondo de la barra de navegación */
    z-index: 999;
  }
}
</style>

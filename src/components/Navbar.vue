<script setup>
import { useRouter } from 'vue-router'; // Importa useRouter en lugar de router
const router = useRouter();
const handlingLogin = () => {
    console.log("aarr")
    console.log(localStorage.getItem("user"));
    //router.push({ name: 'login' });
}


</script>

<template>
    <nav>
        <div>
            <h1>
                All reviews
            </h1>
        </div>
        <div>
            <img src="../assets/loguito.png" alt="">
        </div>
        <div class="div-buttons">
            <button type="button" class="btn btn btn-outline-light" @click="handlingLogin">Login</button>
            <button type="button" class="btn btn btn-light" @click="router.push({ name: 'signup' })">Signup</button>
        </div>
    </nav>
</template>

<style lang="scss">
nav {
    background-color: rgb(26, 26, 26);
    width: 100%;
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 20px;
    color: white;
    .div-buttons{
        display: flex;
        gap: 10px;
    }
    h1{
        font-size:x-large;
        margin: 0;
    }
    img{
        height: 30px;
        width: auto;
    }
}
</style>
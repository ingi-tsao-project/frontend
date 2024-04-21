<script>
import axios from "axios";
export default {
    name: "Login",
    methods: {
        async handleSubmit() {
            console.log("Runing request...");
            const response = await axios.get("tours", {
                email: "batman@gmail.com",
                password: "pro12345"
            });
            console.log(response)
        }
    }
}
</script>

<template>
    <h1>
        asdfsdfs
    </h1>
    <input type="text" name="textoInput">
    <input type="password" id="textoInput" name="textoInput">

    <button @click="handleSubmit">Enviar</button>

</template>

<style scoped></style>

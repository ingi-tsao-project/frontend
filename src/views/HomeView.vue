<script setup>
import TheWelcome from '../components/TheWelcome.vue'
import Navbar from '../components/Navbar.vue'
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

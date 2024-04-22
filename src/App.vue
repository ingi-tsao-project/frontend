<script setup>
import { RouterLink, RouterView } from 'vue-router'
import HelloWorld from './components/HelloWorld.vue'
</script>

<template>
  <div class="main-container">
    <RouterView />
  </div>
</template>

<style scoped>
.main-container {
  width:100%;
  height: 100vh;
  margin:0;
  display:flex;
  align-items: center;
  justify-content: center;
}

</style>

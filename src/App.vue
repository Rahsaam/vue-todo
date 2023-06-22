<script setup>
import { RouterLink, RouterView } from 'vue-router'
</script>

<template>
  <div class="wrapper">
    <header>
      <nav>
        <RouterLink to="/home" class="font-bold text-4xl">home</RouterLink>
      </nav>
    </header>
    
  </div>
  
  <RouterView />
</template>

<script setup>
  
</script>

<style>
  body {
    background: #f2f2f2;
  }
  #app {
    max-width: 600px;
    margin: 0 auto;
    color: #555;
  }
</style>

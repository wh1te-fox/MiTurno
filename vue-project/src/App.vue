<script setup>
import MyButton from './components/main_button.vue'
import main_header from './components/hearder.vue';
</script>


<template>

  <main_header/>

  <!-- Pasando el parámetro "buttonText" -->
  <MyButton buttonText="Enviar Formulario" />
  <MyButton buttonText="Guardar" />

  
  
</template>


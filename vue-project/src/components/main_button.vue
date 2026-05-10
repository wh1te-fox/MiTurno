<script setup>
// Definimos las props que el componente aceptará
defineProps({
  buttonText: {
    type: String,
    default: 'Main Button' // Valor por defecto opcional
  }
})
</script>

<template>
  <button class="bg-blue-500 text-white px-4 py-2 rounded">
    <!-- Usamos la propiedad reactiva aquí -->
    {{ buttonText }}
  </button>
</template>

<script setup>
import { ref } from 'vue'
import BaseButton from '@/components/BaseButton.vue'

const emit = defineEmits(['open-booking'])
const isMobileMenuOpen = ref(false)

function toggleMenu() {
  isMobileMenuOpen.value = !isMobileMenuOpen.value
}
</script>

<template>
  <header class="sticky top-0 z-40 backdrop-blur-md bg-gray-900/95 border-b border-gray-800 text-white shadow-md">
    <nav class="max-w-screen-xl mx-auto px-4 lg:px-6 py-3.5 flex items-center justify-between">
      <a href="#" class="flex items-center space-x-3">
        <img src="@/assets/logoMI.png" class="h-9 w-auto" alt="Logo MiTurno" />
        <span class="self-center text-xl font-extrabold tracking-tight text-white">MiTurno</span>
      </a>

      <div class="hidden lg:flex items-center space-x-8 text-sm font-medium">
        <a href="#especialidades" class="text-gray-300 hover:text-emerald-400 transition-colors">Especialidades</a>
        <a href="#beneficios" class="text-gray-300 hover:text-emerald-400 transition-colors">Beneficios</a>
        <a href="#como-funciona" class="text-gray-300 hover:text-emerald-400 transition-colors">Cómo Funciona</a>
        <a href="#testimonios" class="text-gray-300 hover:text-emerald-400 transition-colors">Testimonios</a>
        <a href="#faq" class="text-gray-300 hover:text-emerald-400 transition-colors">FAQ</a>
      </div>

      <div class="flex items-center space-x-3">
        <button class="hidden sm:inline-flex text-sm font-medium text-gray-300 hover:text-white px-3 py-2 transition-colors">
          Iniciar sesión
        </button>
        <BaseButton buttonText="Reservar Cita" size="sm" @click="$emit('open-booking')" />

        <button 
          @click="toggleMenu"
          type="button" 
          class="inline-flex items-center p-2 text-sm text-gray-400 rounded-lg lg:hidden hover:bg-gray-800 focus:outline-none focus:ring-2 focus:ring-gray-700" 
          :aria-expanded="isMobileMenuOpen"
        >
          <span class="sr-only">Abrir menú principal</span>
          <svg v-if="!isMobileMenuOpen" class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16" />
          </svg>
          <svg v-else class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
          </svg>
        </button>
      </div>
    </nav>

    <!-- Mobile Navigation Menu -->
    <div v-if="isMobileMenuOpen" class="lg:hidden bg-gray-900 border-b border-gray-800 px-4 pt-2 pb-6 space-y-3">
      <a href="#especialidades" @click="isMobileMenuOpen = false" class="block py-2 text-gray-300 hover:text-emerald-400 font-medium">Especialidades</a>
      <a href="#beneficios" @click="isMobileMenuOpen = false" class="block py-2 text-gray-300 hover:text-emerald-400 font-medium">Beneficios</a>
      <a href="#como-funciona" @click="isMobileMenuOpen = false" class="block py-2 text-gray-300 hover:text-emerald-400 font-medium">Cómo Funciona</a>
      <a href="#testimonios" @click="isMobileMenuOpen = false" class="block py-2 text-gray-300 hover:text-emerald-400 font-medium">Testimonios</a>
      <a href="#faq" @click="isMobileMenuOpen = false" class="block py-2 text-gray-300 hover:text-emerald-400 font-medium">FAQ</a>
    </div>
  </header>
</template>

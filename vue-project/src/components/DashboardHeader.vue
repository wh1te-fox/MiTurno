<script setup>
import { ref } from 'vue'

defineProps({
  userName: {
    type: String,
    default: 'María'
  }
})

defineEmits(['toggle-mobile'])

const showNotifications = ref(false)
const showUserMenu = ref(false)

const notifications = [
  { id: 1, title: 'Recordatorio de cita', desc: 'Mañana a las 10:00 AM con Dr. Roberto Gómez', time: 'Hace 1h' },
  { id: 2, title: 'Cita confirmada', desc: 'Tu turno de Dermatología fue agendado exitosamente.', time: 'Ayer' }
]
</script>

<template>
  <header class="sticky top-0 z-30 bg-white/90 backdrop-blur-md border-b border-gray-100 px-6 py-4 flex items-center justify-between shadow-xs">
    <div class="flex items-center space-x-4">
      <button @click="$emit('toggle-mobile')" class="lg:hidden text-gray-600 hover:text-gray-900 focus:outline-none">
        <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16" />
        </svg>
      </button>
      <div>
        <h1 class="text-xl sm:text-2xl font-bold text-gray-900 tracking-tight">Hola, {{ userName }} 👋</h1>
        <p class="text-xs sm:text-sm text-gray-500">Bienvenida a tu panel personal de salud</p>
      </div>
    </div>

    <!-- Right Side Actions -->
    <div class="flex items-center space-x-4 relative">
      <!-- Notifications dropdown -->
      <div class="relative">
        <button 
          @click="showNotifications = !showNotifications; showUserMenu = false"
          class="relative p-2.5 rounded-xl bg-gray-50 hover:bg-gray-100 text-gray-600 transition-colors"
          aria-label="Notificaciones"
        >
          🔔
          <span class="absolute top-1.5 right-1.5 w-2.5 h-2.5 bg-emerald-500 rounded-full ring-2 ring-white"></span>
        </button>

        <div v-if="showNotifications" class="absolute right-0 mt-2 w-80 bg-white rounded-2xl shadow-xl border border-gray-100 py-3 z-50">
          <div class="px-4 pb-2 border-b border-gray-100 flex items-center justify-between">
            <h4 class="font-bold text-gray-900 text-sm">Notificaciones</h4>
            <span class="text-xs text-emerald-600 font-semibold cursor-pointer">Marcar leídas</span>
          </div>
          <div class="divide-y divide-gray-50 max-h-72 overflow-y-auto">
            <div v-for="n in notifications" :key="n.id" class="p-3 hover:bg-gray-50 transition-colors cursor-pointer">
              <p class="text-xs font-bold text-gray-900">{{ n.title }}</p>
              <p class="text-xs text-gray-600 mt-0.5">{{ n.desc }}</p>
              <span class="text-[10px] text-gray-400 mt-1 block">{{ n.time }}</span>
            </div>
          </div>
        </div>
      </div>

      <!-- User Profile Dropdown -->
      <div class="relative">
        <button 
          @click="showUserMenu = !showUserMenu; showNotifications = false"
          class="flex items-center space-x-3 p-1.5 rounded-xl hover:bg-gray-50 transition-colors"
        >
          <div class="w-10 h-10 rounded-full bg-emerald-600 text-white font-bold flex items-center justify-center shadow-sm">
            MG
          </div>
          <span class="hidden sm:inline font-semibold text-sm text-gray-700">María G.</span>
        </button>

        <div v-if="showUserMenu" class="absolute right-0 mt-2 w-56 bg-white rounded-2xl shadow-xl border border-gray-100 py-2 z-50">
          <div class="px-4 py-2 border-b border-gray-100">
            <p class="text-sm font-bold text-gray-900">María González</p>
            <p class="text-xs text-gray-500">ID: #MT-84920</p>
          </div>
          <a href="#" class="block px-4 py-2 text-sm text-gray-700 hover:bg-gray-50">Mi Perfil</a>
          <a href="#" class="block px-4 py-2 text-sm text-gray-700 hover:bg-gray-50">Configuración</a>
          <div class="border-t border-gray-100 my-1"></div>
          <a href="#" class="block px-4 py-2 text-sm text-red-600 hover:bg-red-50">Cerrar sesión</a>
        </div>
      </div>
    </div>
  </header>
</template>

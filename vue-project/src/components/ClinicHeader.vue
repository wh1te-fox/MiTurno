<script setup>
import { ref } from 'vue'

defineEmits(['toggle-mobile', 'open-new-appointment'])

const searchQuery = ref('')
const showNotifications = ref(false)

const alerts = [
  { id: 1, title: 'Conflicto de agenda', desc: 'Dr. Gómez tiene doble reserva a las 11:30 AM', time: 'Hace 5m', type: 'warning' },
  { id: 2, title: 'Paciente en espera prolongada', desc: 'Juan Pérez lleva > 30 min en Sala de Espera', time: 'Hace 12m', type: 'danger' },
  { id: 3, title: 'Nueva cita online', desc: 'María González agendó cita de Cardiología', time: 'Hace 25m', type: 'info' }
]
</script>

<template>
  <header class="sticky top-0 z-30 bg-white/95 backdrop-blur-md border-b border-gray-200 px-6 py-3.5 flex items-center justify-between shadow-xs">
    <div class="flex items-center space-x-4 flex-1 max-w-xl">
      <button @click="$emit('toggle-mobile')" class="lg:hidden text-gray-600 hover:text-gray-900 focus:outline-none">
        <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16" />
        </svg>
      </button>

      <!-- Global Search -->
      <div class="relative w-full max-w-md hidden sm:block">
        <span class="absolute inset-y-0 left-0 flex items-center pl-3.5 text-gray-400">🔍</span>
        <input 
          type="text" 
          v-model="searchQuery"
          placeholder="Buscar pacientes, citas o médicos..." 
          class="w-full pl-10 pr-4 py-2 bg-gray-50 border border-gray-200 rounded-xl text-sm focus:bg-white focus:ring-2 focus:ring-blue-500 focus:outline-none transition-all" 
        />
      </div>
    </div>

    <!-- Right Actions -->
    <div class="flex items-center space-x-3">
      <!-- Quick Action Button -->
      <button 
        @click="$emit('open-new-appointment')"
        class="bg-blue-600 hover:bg-blue-700 active:bg-blue-800 text-white text-sm font-semibold px-4 py-2 rounded-xl shadow-sm transition-all cursor-pointer hidden md:inline-flex items-center space-x-1.5"
      >
        <span>+</span>
        <span>Nueva Cita</span>
      </button>

      <!-- Notifications Dropdown -->
      <div class="relative">
        <button 
          @click="showNotifications = !showNotifications"
          class="relative p-2.5 rounded-xl bg-gray-50 hover:bg-gray-100 text-gray-600 transition-colors cursor-pointer"
          aria-label="Notificaciones"
        >
          🔔
          <span class="absolute top-1.5 right-1.5 w-2.5 h-2.5 bg-red-500 rounded-full ring-2 ring-white"></span>
        </button>

        <div v-if="showNotifications" class="absolute right-0 mt-2 w-80 bg-white rounded-2xl shadow-xl border border-gray-100 py-3 z-50">
          <div class="px-4 pb-2 border-b border-gray-100 flex items-center justify-between">
            <h4 class="font-bold text-gray-900 text-sm">Alertas del Sistema</h4>
            <span class="text-xs text-blue-600 font-semibold cursor-pointer">Marcar leídas</span>
          </div>
          <div class="divide-y divide-gray-50 max-h-72 overflow-y-auto">
            <div v-for="alert in alerts" :key="alert.id" class="p-3 hover:bg-gray-50 transition-colors cursor-pointer">
              <div class="flex items-center space-x-2">
                <span :class="[
                  'w-2 h-2 rounded-full flex-shrink-0',
                  alert.type === 'danger' ? 'bg-red-500' : alert.type === 'warning' ? 'bg-amber-500' : 'bg-blue-500'
                ]"></span>
                <p class="text-xs font-bold text-gray-900">{{ alert.title }}</p>
              </div>
              <p class="text-xs text-gray-600 mt-1 pl-4">{{ alert.desc }}</p>
              <span class="text-[10px] text-gray-400 mt-1 block pl-4">{{ alert.time }}</span>
            </div>
          </div>
        </div>
      </div>

      <!-- User Info -->
      <div class="flex items-center space-x-3 pl-2 border-l border-gray-200">
        <div class="w-9 h-9 rounded-full bg-blue-600 text-white font-bold flex items-center justify-center text-sm shadow-xs">
          CM
        </div>
        <div class="hidden md:block">
          <p class="text-xs font-bold text-gray-900 leading-tight">Carlos Mendoza</p>
          <p class="text-[10px] text-gray-500 font-medium">Administrador</p>
        </div>
      </div>
    </div>
  </header>
</template>

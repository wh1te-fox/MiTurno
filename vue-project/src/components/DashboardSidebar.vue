<script setup>
defineProps({
  currentView: {
    type: String,
    required: true
  },
  isMobileOpen: {
    type: Boolean,
    default: false
  }
})

defineEmits(['navigate', 'close-mobile', 'open-booking'])

const navItems = [
  { id: 'dashboard', label: 'Inicio', icon: '🏠' },
  { id: 'appointments', label: 'Mis citas', icon: '📅' },
  { id: 'history', label: 'Historial', icon: '📋' },
  { id: 'profile', label: 'Perfil', icon: '👤' },
  { id: 'settings', label: 'Configuración', icon: '⚙️' }
]
</script>

<template>
  <!-- Sidebar for Desktop & Mobile Drawer -->
  <aside 
    :class="[
      'fixed inset-y-0 left-0 z-50 w-64 bg-white border-r border-gray-100 shadow-sm flex flex-col transition-transform duration-300 lg:translate-x-0',
      isMobileOpen ? 'translate-x-0' : '-translate-x-full'
    ]"
  >
    <!-- Logo & Brand -->
    <div class="p-6 border-b border-gray-100 flex items-center justify-between">
      <div class="flex items-center space-x-3">
        <img src="@/assets/logoMI.png" class="h-8 w-auto" alt="Logo MiTurno" />
        <span class="text-xl font-bold tracking-tight text-gray-900">MiTurno</span>
      </div>
      <button @click="$emit('close-mobile')" class="lg:hidden text-gray-400 hover:text-gray-600">
        ✕
      </button>
    </div>

    <!-- Quick Action: Agendar nueva cita -->
    <div class="p-4">
      <button 
        @click="$emit('open-booking')"
        class="w-full bg-emerald-600 hover:bg-emerald-700 active:bg-emerald-800 text-white font-semibold py-3 px-4 rounded-xl shadow-md transition-all duration-200 flex items-center justify-center space-x-2 cursor-pointer"
      >
        <span>+</span>
        <span>Agendar nueva cita</span>
      </button>
    </div>

    <!-- Navigation Links -->
    <nav class="flex-1 px-4 py-2 space-y-1 overflow-y-auto">
      <button
        v-for="item in navItems"
        :key="item.id"
        @click="$emit('navigate', item.id)"
        :class="[
          'w-full flex items-center space-x-3 px-4 py-3 rounded-xl font-medium text-sm transition-colors text-left cursor-pointer',
          currentView === item.id 
            ? 'bg-emerald-50 text-emerald-700 font-semibold' 
            : 'text-gray-600 hover:bg-gray-50 hover:text-gray-900'
        ]"
      >
        <span class="text-lg">{{ item.icon }}</span>
        <span>{{ item.label }}</span>
      </button>
    </nav>

    <!-- Patient quick card / Footer in sidebar -->
    <div class="p-4 border-t border-gray-100">
      <div class="bg-gray-50 p-3 rounded-xl flex items-center space-x-3">
        <div class="w-10 h-10 rounded-full bg-emerald-100 text-emerald-700 font-bold flex items-center justify-center">
          M
        </div>
        <div class="overflow-hidden">
          <p class="text-sm font-semibold text-gray-900 truncate">María González</p>
          <p class="text-xs text-gray-500 truncate">maria@ejemplo.com</p>
        </div>
      </div>
    </div>
  </aside>
</template>

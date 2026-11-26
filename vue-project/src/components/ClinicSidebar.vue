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

defineEmits(['navigate', 'close-mobile'])

const navItems = [
  { id: 'dashboard', label: 'Dashboard', icon: '📊' },
  { id: 'agenda', label: 'Agenda', icon: '📅' },
  { id: 'appointments', label: 'Citas', icon: '📝' },
  { id: 'patients', label: 'Pacientes', icon: '👥' },
  { id: 'waiting-room', label: 'Sala de espera', icon: '⏳' },
  { id: 'staff', label: 'Médicos y personal', icon: '🩺' },
  { id: 'reports', label: 'Reportes', icon: '📈' },
  { id: 'settings', label: 'Configuración', icon: '⚙️' }
]
</script>

<template>
  <aside 
    :class="[
      'fixed inset-y-0 left-0 z-50 w-64 bg-white border-r border-gray-200 flex flex-col transition-transform duration-300 lg:translate-x-0 shadow-sm',
      isMobileOpen ? 'translate-x-0' : '-translate-x-full'
    ]"
  >
    <!-- Logo & Clinic Name -->
    <div class="p-6 border-b border-gray-100 flex items-center justify-between">
      <div class="flex items-center space-x-3">
        <img src="@/assets/logoMI.png" class="h-8 w-auto" alt="Logo MiTurno" />
        <div>
          <span class="text-base font-extrabold tracking-tight text-gray-950 block leading-none">MiTurno</span>
          <span class="text-[11px] font-semibold text-blue-600 uppercase tracking-wider">Clínica Central</span>
        </div>
      </div>
      <button @click="$emit('close-mobile')" class="lg:hidden text-gray-400 hover:text-gray-600">
        ✕
      </button>
    </div>

    <!-- Navigation -->
    <nav class="flex-1 px-4 py-4 space-y-1 overflow-y-auto">
      <button
        v-for="item in navItems"
        :key="item.id"
        @click="$emit('navigate', item.id)"
        :class="[
          'w-full flex items-center space-x-3 px-4 py-3 rounded-xl font-medium text-sm transition-colors text-left cursor-pointer',
          currentView === item.id 
            ? 'bg-blue-600 text-white font-semibold shadow-sm' 
            : 'text-gray-600 hover:bg-gray-50 hover:text-gray-900'
        ]"
      >
        <span class="text-lg">{{ item.icon }}</span>
        <span>{{ item.label }}</span>
      </button>
    </nav>

    <!-- User Role Badge in Sidebar Footer -->
    <div class="p-4 border-t border-gray-100">
      <div class="bg-gray-50 p-3 rounded-xl flex items-center space-x-3">
        <div class="w-9 h-9 rounded-full bg-blue-100 text-blue-700 font-bold flex items-center justify-center text-sm">
          CM
        </div>
        <div class="overflow-hidden">
          <p class="text-xs font-bold text-gray-900 truncate">Carlos Mendoza</p>
          <p class="text-[10px] text-blue-600 font-semibold truncate">Administrador</p>
        </div>
      </div>
    </div>
  </aside>
</template>

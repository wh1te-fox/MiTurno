<script setup>
defineProps({
  isOpen: {
    type: Boolean,
    default: false
  },
  appointment: {
    type: Object,
    default: null
  }
})

const emit = defineEmits(['close', 'change-status', 'cancel-appointment'])

const statuses = ['Confirmada', 'En espera', 'En consulta', 'Completada', 'Cancelada', 'No asistió']
</script>

<template>
  <div v-if="isOpen && appointment" class="fixed inset-0 z-50 flex items-center justify-center p-4 bg-black/60 backdrop-blur-sm">
    <div class="bg-white rounded-3xl shadow-2xl w-full max-w-lg overflow-hidden border border-gray-100">
      <div class="bg-gray-900 px-6 py-4 text-white flex items-center justify-between">
        <div>
          <h3 class="font-bold text-lg">Detalles y Gestión de Cita</h3>
          <p class="text-xs text-gray-400">ID: #MT-{{ 1000 + appointment.id }}</p>
        </div>
        <button @click="$emit('close')" class="text-gray-400 hover:text-white">✕</button>
      </div>

      <div class="p-6 space-y-5">
        <div class="bg-blue-50 p-4 rounded-2xl flex items-center justify-between">
          <div>
            <h4 class="font-bold text-gray-900 text-lg">{{ appointment.patientName }}</h4>
            <p class="text-xs text-blue-700 font-semibold">{{ appointment.specialty }} • {{ appointment.doctor }}</p>
          </div>
          <span class="px-3 py-1 bg-blue-600 text-white font-bold text-xs rounded-full">
            {{ appointment.status }}
          </span>
        </div>

        <div class="grid grid-cols-2 gap-4 text-sm text-gray-600">
          <div class="p-3 bg-gray-50 rounded-xl">
            <span class="text-xs text-gray-400 block font-semibold">Horario</span>
            <span class="font-bold text-gray-900">{{ appointment.time }}</span>
          </div>
          <div class="p-3 bg-gray-50 rounded-xl">
            <span class="text-xs text-gray-400 block font-semibold">Consultorio</span>
            <span class="font-bold text-gray-900">{{ appointment.consultorio }}</span>
          </div>
        </div>

        <!-- Quick Status Change -->
        <div>
          <label class="block text-xs font-bold text-gray-700 uppercase tracking-wider mb-2">Cambiar Estado de la Consulta</label>
          <div class="grid grid-cols-3 gap-2">
            <button 
              v-for="st in statuses" 
              :key="st"
              @click="$emit('change-status', { appointment, status: st })"
              :class="[
                'py-2 px-2.5 rounded-xl text-xs font-semibold border transition-all',
                appointment.status === st 
                  ? 'bg-blue-600 text-white border-blue-600 shadow-xs' 
                  : 'bg-white text-gray-700 border-gray-200 hover:bg-gray-50'
              ]"
            >
              {{ st }}
            </button>
          </div>
        </div>

        <div class="pt-4 border-t border-gray-100 flex justify-between items-center">
          <button 
            @click="$emit('cancel-appointment', appointment)"
            class="text-red-600 hover:text-red-800 text-xs font-bold px-3 py-2 rounded-lg hover:bg-red-50 transition-colors"
          >
            Cancelar Cita
          </button>
          <button 
            @click="$emit('close')" 
            class="px-5 py-2.5 bg-gray-900 text-white rounded-xl text-xs font-bold hover:bg-black"
          >
            Cerrar
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

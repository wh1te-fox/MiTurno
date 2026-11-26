<script setup>
import { reactive } from 'vue'

defineProps({
  isOpen: {
    type: Boolean,
    default: false
  }
})

const emit = defineEmits(['close', 'create'])

const form = reactive({
  patientName: '',
  doctor: 'Dr. Roberto Gómez',
  specialty: 'Cardiología',
  consultorio: 'Consultorio 402',
  date: '2026-10-06',
  time: '11:00 AM',
  type: 'Presencial'
})

function submitForm() {
  if (!form.patientName) return
  emit('create', { ...form, id: Date.now(), status: 'Confirmada' })
  form.patientName = ''
  emit('close')
}
</script>

<template>
  <div v-if="isOpen" class="fixed inset-0 z-50 flex items-center justify-center p-4 bg-black/60 backdrop-blur-sm">
    <div class="bg-white rounded-3xl shadow-2xl w-full max-w-lg overflow-hidden border border-gray-100">
      <div class="bg-blue-600 px-6 py-4 text-white flex items-center justify-between">
        <h3 class="font-bold text-lg">Programar Nueva Cita Médica</h3>
        <button @click="$emit('close')" class="text-white/80 hover:text-white">✕</button>
      </div>

      <form @submit.prevent="submitForm" class="p-6 space-y-4">
        <div>
          <label class="block text-xs font-semibold text-gray-600 uppercase mb-1">Nombre del Paciente</label>
          <input type="text" v-model="form.patientName" required placeholder="Ej. Camila Herrera" class="w-full px-4 py-2.5 rounded-xl border border-gray-300 focus:ring-2 focus:ring-blue-500 focus:outline-none" />
        </div>

        <div class="grid grid-cols-2 gap-4">
          <div>
            <label class="block text-xs font-semibold text-gray-600 uppercase mb-1">Especialidad</label>
            <select v-model="form.specialty" class="w-full px-4 py-2.5 rounded-xl border border-gray-300 bg-white">
              <option>Cardiología</option>
              <option>Pediatría</option>
              <option>Dermatología</option>
              <option>Medicina General</option>
              <option>Oftalmología</option>
            </select>
          </div>
          <div>
            <label class="block text-xs font-semibold text-gray-600 uppercase mb-1">Médico Asignado</label>
            <select v-model="form.doctor" class="w-full px-4 py-2.5 rounded-xl border border-gray-300 bg-white">
              <option>Dr. Roberto Gómez</option>
              <option>Dra. Sofía Martínez</option>
              <option>Dr. Alejandro Silva</option>
              <option>Dr. Mario Vargas</option>
            </select>
          </div>
        </div>

        <div class="grid grid-cols-2 gap-4">
          <div>
            <label class="block text-xs font-semibold text-gray-600 uppercase mb-1">Fecha</label>
            <input type="date" v-model="form.date" class="w-full px-4 py-2.5 rounded-xl border border-gray-300" />
          </div>
          <div>
            <label class="block text-xs font-semibold text-gray-600 uppercase mb-1">Hora</label>
            <input type="text" v-model="form.time" placeholder="Ej. 11:30 AM" class="w-full px-4 py-2.5 rounded-xl border border-gray-300" />
          </div>
        </div>

        <div class="grid grid-cols-2 gap-4">
          <div>
            <label class="block text-xs font-semibold text-gray-600 uppercase mb-1">Consultorio</label>
            <select v-model="form.consultorio" class="w-full px-4 py-2.5 rounded-xl border border-gray-300 bg-white">
              <option>Consultorio 101</option>
              <option>Consultorio 201</option>
              <option>Consultorio 402</option>
              <option>Consultorio 308</option>
            </select>
          </div>
          <div>
            <label class="block text-xs font-semibold text-gray-600 uppercase mb-1">Modalidad</label>
            <select v-model="form.type" class="w-full px-4 py-2.5 rounded-xl border border-gray-300 bg-white">
              <option>Presencial</option>
              <option>Virtual / Telemedicina</option>
            </select>
          </div>
        </div>

        <div class="pt-4 flex justify-end space-x-3">
          <button type="button" @click="$emit('close')" class="px-4 py-2.5 rounded-xl border border-gray-300 text-gray-700 font-semibold text-sm">
            Cancelar
          </button>
          <button type="submit" class="px-5 py-2.5 rounded-xl bg-blue-600 hover:bg-blue-700 text-white font-semibold text-sm shadow-sm">
            Confirmar y Guardar Cita
          </button>
        </div>
      </form>
    </div>
  </div>
</template>

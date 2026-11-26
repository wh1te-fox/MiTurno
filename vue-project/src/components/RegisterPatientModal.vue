<script setup>
import { reactive } from 'vue'

defineProps({
  isOpen: {
    type: Boolean,
    default: false
  }
})

const emit = defineEmits(['close', 'registered'])

const form = reactive({
  name: '',
  doc: '',
  phone: '',
  dob: '',
  email: ''
})

function submit() {
  if (!form.name || !form.doc) return
  emit('registered', { ...form, id: Date.now() })
  emit('close')
}
</script>

<template>
  <div v-if="isOpen" class="fixed inset-0 z-50 flex items-center justify-center p-4 bg-black/60 backdrop-blur-sm">
    <div class="bg-white rounded-3xl shadow-2xl w-full max-w-lg overflow-hidden border border-gray-100">
      <div class="bg-blue-600 px-6 py-4 text-white flex items-center justify-between">
        <h3 class="font-bold text-lg">Registrar Nuevo Paciente</h3>
        <button @click="$emit('close')" class="text-white/80 hover:text-white">✕</button>
      </div>

      <form @submit.prevent="submit" class="p-6 space-y-4">
        <div>
          <label class="block text-xs font-semibold text-gray-600 uppercase mb-1">Nombre Completo</label>
          <input type="text" v-model="form.name" required placeholder="Ej. Camila Herrera" class="w-full px-4 py-2.5 rounded-xl border border-gray-300 focus:ring-2 focus:ring-blue-500 focus:outline-none" />
        </div>

        <div class="grid grid-cols-2 gap-4">
          <div>
            <label class="block text-xs font-semibold text-gray-600 uppercase mb-1">Documento / ID</label>
            <input type="text" v-model="form.doc" required placeholder="39.882.119" class="w-full px-4 py-2.5 rounded-xl border border-gray-300" />
          </div>
          <div>
            <label class="block text-xs font-semibold text-gray-600 uppercase mb-1">Fecha de Nacimiento</label>
            <input type="date" v-model="form.dob" class="w-full px-4 py-2.5 rounded-xl border border-gray-300" />
          </div>
        </div>

        <div class="grid grid-cols-2 gap-4">
          <div>
            <label class="block text-xs font-semibold text-gray-600 uppercase mb-1">Teléfono</label>
            <input type="tel" v-model="form.phone" placeholder="+54 9 11 ..." class="w-full px-4 py-2.5 rounded-xl border border-gray-300" />
          </div>
          <div>
            <label class="block text-xs font-semibold text-gray-600 uppercase mb-1">Correo Electrónico</label>
            <input type="email" v-model="form.email" placeholder="paciente@correo.com" class="w-full px-4 py-2.5 rounded-xl border border-gray-300" />
          </div>
        </div>

        <div class="pt-4 flex justify-end space-x-3">
          <button type="button" @click="$emit('close')" class="px-4 py-2.5 rounded-xl border border-gray-300 text-gray-700 font-semibold text-sm">
            Cancelar
          </button>
          <button type="submit" class="px-5 py-2.5 rounded-xl bg-blue-600 hover:bg-blue-700 text-white font-semibold text-sm shadow-sm">
            Registrar y Abrir Ficha
          </button>
        </div>
      </form>
    </div>
  </div>
</template>

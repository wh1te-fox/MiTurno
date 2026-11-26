<script setup>
import { reactive } from 'vue'
import BaseButton from '@/components/BaseButton.vue'

const profile = reactive({
  name: 'María González',
  email: 'maria.gonzalez@ejemplo.com',
  phone: '+54 9 11 4455-6677',
  birthdate: '1992-05-14',
  idNumber: '38.492.102'
})

function saveProfile() {
  alert('Perfil actualizado correctamente.')
}
</script>

<template>
  <div class="space-y-6 pb-12 max-w-3xl">
    <div>
      <h2 class="text-2xl font-bold text-gray-900">Perfil del Paciente</h2>
      <p class="text-sm text-gray-500">Actualiza tu información personal y datos de contacto.</p>
    </div>

    <div class="bg-white p-8 rounded-3xl shadow-sm border border-gray-100 space-y-6">
      <div class="flex items-center space-x-6 pb-6 border-b border-gray-100">
        <div class="w-20 h-20 rounded-full bg-emerald-600 text-white font-bold text-2xl flex items-center justify-center shadow-md">
          MG
        </div>
        <div>
          <h3 class="font-bold text-lg text-gray-900">{{ profile.name }}</h3>
          <p class="text-xs text-gray-500">ID Paciente: #MT-84920</p>
          <button class="text-xs font-semibold text-emerald-600 hover:underline mt-1 block">Cambiar foto de perfil</button>
        </div>
      </div>

      <div class="grid grid-cols-1 sm:grid-cols-2 gap-6">
        <div>
          <label class="block text-xs font-semibold text-gray-600 uppercase mb-1">Nombre Completo</label>
          <input type="text" v-model="profile.name" class="w-full px-4 py-2.5 rounded-xl border border-gray-300 focus:ring-2 focus:ring-emerald-500" />
        </div>
        <div>
          <label class="block text-xs font-semibold text-gray-600 uppercase mb-1">Correo Electrónico</label>
          <input type="email" v-model="profile.email" class="w-full px-4 py-2.5 rounded-xl border border-gray-300 focus:ring-2 focus:ring-emerald-500" />
        </div>
        <div>
          <label class="block text-xs font-semibold text-gray-600 uppercase mb-1">Teléfono Móvil</label>
          <input type="tel" v-model="profile.phone" class="w-full px-4 py-2.5 rounded-xl border border-gray-300 focus:ring-2 focus:ring-emerald-500" />
        </div>
        <div>
          <label class="block text-xs font-semibold text-gray-600 uppercase mb-1">Fecha de Nacimiento</label>
          <input type="date" v-model="profile.birthdate" class="w-full px-4 py-2.5 rounded-xl border border-gray-300 focus:ring-2 focus:ring-emerald-500" />
        </div>
      </div>

      <div class="pt-4 flex justify-end">
        <BaseButton buttonText="Guardar Cambios" @click="saveProfile" />
      </div>
    </div>
  </div>
</template>

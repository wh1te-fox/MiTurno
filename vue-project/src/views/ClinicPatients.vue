<script setup>
const patients = [
  { id: 1, name: 'María González', doc: '38.492.102', phone: '+54 9 11 4455-6677', dob: '14 May 1992', lastApp: '12 Ene 2026', nextApp: '07 Oct 2026' },
  { id: 2, name: 'Juan Pérez', doc: '32.112.998', phone: '+54 9 11 2233-4455', dob: '22 Nov 1985', lastApp: '10 Feb 2026', nextApp: '08 Oct 2026' },
  { id: 3, name: 'Lucía Torres', doc: '41.223.334', phone: '+54 9 11 9988-7766', dob: '03 Jul 1998', lastApp: '05 Mar 2026', nextApp: '09 Oct 2026' }
]

defineEmits(['register-patient'])
</script>

<template>
  <div class="space-y-6 pb-12">
    <div class="flex items-center justify-between">
      <div>
        <h2 class="text-2xl font-bold text-gray-900">Directorio de Pacientes</h2>
        <p class="text-sm text-gray-500">Administra expedientes, historial de citas e información de pacientes.</p>
      </div>
      <button @click="$emit('register-patient')" class="bg-blue-600 hover:bg-blue-700 text-white text-sm font-semibold px-4 py-2 rounded-xl">
        + Registrar Paciente
      </button>
    </div>

    <div class="bg-white rounded-3xl shadow-xs border border-gray-100 overflow-hidden">
      <div class="overflow-x-auto">
        <table class="w-full text-left border-collapse">
          <thead>
            <tr class="bg-gray-50 text-xs font-semibold text-gray-500 uppercase tracking-wider border-b border-gray-100">
              <th class="py-3.5 px-6">Paciente</th>
              <th class="py-3.5 px-6">Documento / ID</th>
              <th class="py-3.5 px-6">Teléfono</th>
              <th class="py-3.5 px-6">Fecha Nacimiento</th>
              <th class="py-3.5 px-6">Próxima Cita</th>
              <th class="py-3.5 px-6 text-right">Expediente</th>
            </tr>
          </thead>
          <tbody class="divide-y divide-gray-100 text-sm">
            <tr v-for="pat in patients" :key="pat.id" class="hover:bg-gray-50 transition-colors">
              <td class="py-4 px-6 font-bold text-gray-900">{{ pat.name }}</td>
              <td class="py-4 px-6 text-gray-600">{{ pat.doc }}</td>
              <td class="py-4 px-6 text-gray-600">{{ pat.phone }}</td>
              <td class="py-4 px-6 text-gray-600">{{ pat.dob }}</td>
              <td class="py-4 px-6 font-semibold text-blue-600">{{ pat.nextApp }}</td>
              <td class="py-4 px-6 text-right">
                <button class="text-blue-600 hover:underline font-semibold text-xs bg-blue-50 px-3 py-1.5 rounded-lg">Ver perfil</button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

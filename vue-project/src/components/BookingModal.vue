<script setup>
import { ref, reactive, watch } from 'vue'
import BaseButton from '@/components/BaseButton.vue'

const props = defineProps({
  isOpen: {
    type: Boolean,
    default: false
  },
  initialSpecialty: {
    type: String,
    default: ''
  }
})

const emit = defineEmits(['close'])

const step = ref(1)

const specialties = [
  { id: 'cardio', name: 'Cardiología', doctors: ['Dr. Roberto Gómez', 'Dra. Elena Ruiz'] },
  { id: 'pediatria', name: 'Pediatría', doctors: ['Dra. Sofía Martínez', 'Dr. Carlos Mendoza'] },
  { id: 'dermatologia', name: 'Dermatología', doctors: ['Dr. Alejandro Silva', 'Dra. Lucía Torres'] },
  { id: 'general', name: 'Medicina General', doctors: ['Dr. Juan Pérez', 'Dra. María Benítez'] },
  { id: 'odontologia', name: 'Odontología', doctors: ['Dra. Carmen Ríos', 'Dr. Esteban Morales'] },
  { id: 'oftalmologia', name: 'Oftalmología', doctors: ['Dr. Mario Vargas', 'Dra. Patricia León'] }
]

const availableTimes = ['09:00 AM', '10:30 AM', '12:00 PM', '03:30 PM', '05:00 PM']

const form = reactive({
  specialty: '',
  doctor: '',
  date: '',
  time: '',
  name: '',
  email: '',
  phone: ''
})

const currentDoctors = ref([])

function applySpecialty(name) {
  const spec = specialties.find(s => s.name === name) || specialties[0]
  form.specialty = spec.name
  currentDoctors.value = spec.doctors
  if (!spec.doctors.includes(form.doctor)) {
    form.doctor = spec.doctors[0] || ''
  }
}

watch(
  () => [props.isOpen, props.initialSpecialty],
  ([isOpen, specialty]) => {
    if (isOpen) applySpecialty(specialty)
  },
  { immediate: true }
)

function onSpecialtyChange() {
  const spec = specialties.find(s => s.name === form.specialty)
  currentDoctors.value = spec ? spec.doctors : []
  form.doctor = currentDoctors.value[0] || ''
}

function nextStep() {
  if (step.value < 4) step.value++
}

function prevStep() {
  if (step.value > 1) step.value--
}

function submitBooking() {
  step.value = 4 // Success step
}

function closeModal() {
  step.value = 1
  emit('close')
}
</script>

<template>
  <div v-if="isOpen" class="fixed inset-0 z-50 flex items-center justify-center p-4 bg-black/60 backdrop-blur-sm animate-fadeIn">
    <div class="bg-white rounded-2xl shadow-2xl w-full max-w-lg overflow-hidden border border-gray-100">
      <!-- Modal Header -->
      <div class="bg-gradient-to-r from-emerald-600 to-teal-700 px-6 py-4 text-white flex items-center justify-between">
        <div>
          <h3 class="text-lg font-bold">Reservar Cita Médica</h3>
          <p class="text-xs text-emerald-100">{{ step <= 3 ? `Paso ${step} de 3` : 'Reserva confirmada' }} - MiTurno</p>
        </div>
        <button @click="closeModal" class="text-white/80 hover:text-white p-1 rounded-lg hover:bg-white/10 transition-colors">
          <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
          </svg>
        </button>
      </div>

      <!-- Modal Body -->
      <div class="p-6">
        <!-- Step 1: Specialty & Doctor -->
        <div v-if="step === 1" class="space-y-4">
          <h4 class="font-semibold text-gray-800">Selecciona la Especialidad y Profesional</h4>
          
          <div>
            <label class="block text-xs font-semibold text-gray-600 uppercase mb-1">Especialidad Médica</label>
            <select v-model="form.specialty" @change="onSpecialtyChange" class="w-full px-4 py-2.5 rounded-xl border border-gray-300 focus:ring-2 focus:ring-emerald-500 focus:border-emerald-500 bg-white">
              <option v-for="spec in specialties" :key="spec.id" :value="spec.name">{{ spec.name }}</option>
            </select>
          </div>

          <div>
            <label class="block text-xs font-semibold text-gray-600 uppercase mb-1">Médico Especialista</label>
            <select v-model="form.doctor" class="w-full px-4 py-2.5 rounded-xl border border-gray-300 focus:ring-2 focus:ring-emerald-500 focus:border-emerald-500 bg-white">
              <option v-for="doc in currentDoctors" :key="doc" :value="doc">{{ doc }}</option>
            </select>
          </div>

          <div class="pt-4 flex justify-end">
            <BaseButton buttonText="Continuar" @click="nextStep" />
          </div>
        </div>

        <!-- Step 2: Date & Time -->
        <div v-if="step === 2" class="space-y-4">
          <h4 class="font-semibold text-gray-800">Elige Fecha y Horario Disponible</h4>
          
          <div>
            <label class="block text-xs font-semibold text-gray-600 uppercase mb-1">Fecha de la Cita</label>
            <input type="date" v-model="form.date" class="w-full px-4 py-2.5 rounded-xl border border-gray-300 focus:ring-2 focus:ring-emerald-500 focus:border-emerald-500" />
          </div>

          <div>
            <label class="block text-xs font-semibold text-gray-600 uppercase mb-1">Horario</label>
            <div class="grid grid-cols-3 gap-2 mt-2">
              <button 
                v-for="time in availableTimes" 
                :key="time"
                type="button"
                @click="form.time = time"
                :class="[
                  'py-2.5 px-3 rounded-xl text-sm font-medium border transition-all',
                  form.time === time 
                    ? 'bg-emerald-600 text-white border-emerald-600 shadow-sm' 
                    : 'bg-white text-gray-700 border-gray-200 hover:border-emerald-500'
                ]"
              >
                {{ time }}
              </button>
            </div>
          </div>

          <div class="pt-4 flex justify-between">
            <BaseButton buttonText="Atrás" variant="outline" @click="prevStep" />
            <BaseButton buttonText="Continuar" :disabled="!form.date || !form.time" @click="nextStep" />
          </div>
        </div>

        <!-- Step 3: Patient Info -->
        <div v-if="step === 3" class="space-y-4">
          <h4 class="font-semibold text-gray-800">Datos del Paciente</h4>
          
          <div>
            <label class="block text-xs font-semibold text-gray-600 uppercase mb-1">Nombre Completo</label>
            <input type="text" v-model="form.name" placeholder="Ej. Juan Pérez" class="w-full px-4 py-2.5 rounded-xl border border-gray-300 focus:ring-2 focus:ring-emerald-500 focus:border-emerald-500" />
          </div>

          <div>
            <label class="block text-xs font-semibold text-gray-600 uppercase mb-1">Correo Electrónico</label>
            <input type="email" v-model="form.email" placeholder="correo@ejemplo.com" class="w-full px-4 py-2.5 rounded-xl border border-gray-300 focus:ring-2 focus:ring-emerald-500 focus:border-emerald-500" />
          </div>

          <div>
            <label class="block text-xs font-semibold text-gray-600 uppercase mb-1">Teléfono Móvil</label>
            <input type="tel" v-model="form.phone" placeholder="+54 9 11 1234 5678" class="w-full px-4 py-2.5 rounded-xl border border-gray-300 focus:ring-2 focus:ring-emerald-500 focus:border-emerald-500" />
          </div>

          <div class="pt-4 flex justify-between">
            <BaseButton buttonText="Atrás" variant="outline" @click="prevStep" />
            <BaseButton buttonText="Confirmar Reserva" :disabled="!form.name || !form.email" @click="submitBooking" />
          </div>
        </div>

        <!-- Step 4: Success confirmation -->
        <div v-if="step === 4" class="text-center py-6 space-y-4">
          <div class="w-16 h-16 bg-emerald-100 text-emerald-600 rounded-full flex items-center justify-center mx-auto text-3xl font-bold">
            ✓
          </div>
          <h4 class="text-xl font-bold text-gray-900">¡Cita Reservada con Éxito!</h4>
          <p class="text-sm text-gray-600 max-w-sm mx-auto">
            Hemos enviado los detalles de tu turno con <span class="font-semibold text-gray-800">{{ form.doctor }}</span> ({{ form.specialty }}) para el día <span class="font-semibold text-gray-800">{{ form.date }}</span> a las <span class="font-semibold text-gray-800">{{ form.time }}</span> a tu correo <span class="font-semibold text-gray-800">{{ form.email }}</span>.
          </p>
          <div class="pt-4">
            <BaseButton buttonText="Finalizar" block @click="closeModal" />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

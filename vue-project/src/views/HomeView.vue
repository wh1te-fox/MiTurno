<script setup>
import { ref } from 'vue'
import BaseButton from '@/components/BaseButton.vue'
import BookingModal from '@/components/BookingModal.vue'
import AppFooter from '@/components/AppFooter.vue'

const isBookingOpen = ref(false)
const selectedSpecialty = ref('Cardiología')

const searchSpecialty = ref('')
const searchLocation = ref('')

const specialties = [
  { id: 1, name: 'Cardiología', icon: '❤️', desc: 'Cuidado integral de tu corazón y sistema circulatorio con especialistas top.', doctorsCount: '12 especialistas' },
  { id: 2, name: 'Pediatría', icon: '👶', desc: 'Atención cálida y profesional para el crecimiento y salud de tus hijos.', doctorsCount: '15 especialistas' },
  { id: 3, name: 'Dermatología', icon: '✨', desc: 'Salud de la piel, diagnóstico temprano y tratamientos dermatológicos avanzados.', doctorsCount: '8 especialistas' },
  { id: 4, name: 'Medicina General', icon: '🩺', desc: 'Consulta primaria, chequeos preventivos y derivación experta.', doctorsCount: '20 especialistas' },
  { id: 5, name: 'Odontología', icon: '🦷', desc: 'Estética dental, ortodoncia y salud bucal sin dolor ni esperas.', doctorsCount: '10 especialistas' },
  { id: 6, name: 'Oftalmología', icon: '👁️', desc: 'Exámenes visuales completos, cirugía láser y corrección óptica.', doctorsCount: '9 especialistas' }
]

const steps = [
  { num: '01', title: 'Busca tu Especialista', desc: 'Filtra por especialidad médica, ubicación o profesional de confianza.' },
  { num: '02', title: 'Elige Día y Hora', desc: 'Visualiza la agenda en tiempo real y selecciona el turno que mejor se adapte a ti.' },
  { num: '03', title: 'Confirma y Asiste', desc: 'Recibe recordatorios automáticos por correo y acude a tu consulta sin filas.' }
]

const testimonials = [
  { name: 'Valeria Martínez', role: 'Paciente verificado', comment: '¡Excelente plataforma! Reservé turno con un cardiólogo en menos de 2 minutos sin tener que llamar por teléfono.', rating: '★★★★★' },
  { name: 'Dr. Esteban Ruiz', role: 'Médico Pediatra', comment: 'MiTurno ha optimizado la gestión de mi consultorio médico, reduciendo ausencias y mejorando la atención a mis pacientes.', rating: '★★★★★' },
  { name: 'Lucas Giménez', role: 'Paciente verificado', comment: 'La interfaz es súper limpia e intuitiva. Los recordatorios automáticos me salvan siempre.', rating: '★★★★★' }
]

const faqs = [
  { q: '¿Es gratuito utilizar MiTurno?', a: 'Sí, para los pacientes el servicio de búsqueda y reserva de turnos médicos es 100% gratuito.' },
  { q: '¿Cómo recibo la confirmación de mi turno?', a: 'Recibirás un correo electrónico con todos los detalles de tu cita, incluyendo dirección, hora y recomendaciones previas.' },
  { q: '¿Puedo cancelar o reprogramar una cita?', a: 'Por supuesto. Desde tu panel o correo de confirmación puedes gestionar tus turnos de forma autónoma con al menos 24 horas de anticipación.' },
  { q: '¿Trabajan con obras sociales y prepagas?', a: 'Sí, cada profesional e institución indica en su perfil las obras sociales y prepagas con las que opera.' }
]

function openBooking(specialty = 'Cardiología') {
  selectedSpecialty.value = specialty
  isBookingOpen.value = true
}
</script>

<template>
  <div class="min-h-screen bg-white text-gray-800 flex flex-col font-sans">
    
    <!-- Hero Section -->
    <section class="relative bg-gradient-to-br from-gray-900 via-gray-800 to-emerald-950 text-white overflow-hidden py-20 lg:py-28">
      <!-- Background decorative glow -->
      <div class="absolute -top-40 -right-40 w-96 h-96 bg-emerald-500/20 rounded-full blur-3xl pointer-events-none"></div>
      <div class="absolute -bottom-40 -left-40 w-96 h-96 bg-teal-500/20 rounded-full blur-3xl pointer-events-none"></div>

      <div class="max-w-screen-xl mx-auto px-4 lg:px-6 relative z-10">
        <div class="grid grid-cols-1 lg:grid-cols-12 gap-12 items-center">
          
          <div class="lg:col-span-7 space-y-6">
            <span class="inline-flex items-center space-x-2 px-3.5 py-1.5 text-xs font-semibold tracking-wide text-emerald-400 uppercase bg-emerald-950/80 border border-emerald-800/60 rounded-full shadow-inner">
              <span class="w-2 h-2 rounded-full bg-emerald-400 animate-pulse"></span>
              Plataforma Médica Inteligente
            </span>
            <h1 class="text-4xl sm:text-6xl font-black tracking-tight leading-tight">
              Tu Salud, <span class="text-transparent bg-clip-text bg-gradient-to-r from-emerald-400 to-teal-300">Nuestra Prioridad</span>
            </h1>
            <p class="text-lg sm:text-xl text-gray-300 max-w-2xl leading-relaxed">
              Agenda, administra y consulta tus turnos médicos fácilmente. Conéctate con los mejores profesionales organizando una experiencia de salud sin complicaciones.
            </p>

            <div class="flex flex-wrap gap-4 pt-2">
              <BaseButton buttonText="Reservar Cita Ahora" size="lg" @click="openBooking()" />
              <BaseButton buttonText="Ver Especialidades" variant="outline" size="lg" class="border-gray-600 text-white hover:bg-gray-800" />
            </div>

            <div class="grid grid-cols-3 gap-6 pt-8 border-t border-gray-800/80 text-gray-400 text-sm">
              <div>
                <p class="text-2xl font-bold text-white">+500</p>
                <p>Especialistas</p>
              </div>
              <div>
                <p class="text-2xl font-bold text-white">+50k</p>
                <p>Turnos Dados</p>
              </div>
              <div>
                <p class="text-2xl font-bold text-white">99%</p>
                <p>Satisfacción</p>
              </div>
            </div>
          </div>

          <!-- Hero Search Card Widget -->
          <div class="lg:col-span-5">
            <div class="bg-white/10 backdrop-blur-xl border border-white/20 p-6 sm:p-8 rounded-3xl shadow-2xl space-y-5">
              <h3 class="text-xl font-bold text-white">Búsqueda Rápida de Turnos</h3>
              
              <div class="space-y-4">
                <div>
                  <label class="block text-xs font-semibold text-emerald-300 uppercase mb-1">Especialidad o Profesional</label>
                  <input type="text" v-model="searchSpecialty" placeholder="Ej. Cardiología, Pediatría..." class="w-full px-4 py-3 rounded-xl bg-gray-900/80 border border-gray-700 text-white placeholder-gray-500 focus:ring-2 focus:ring-emerald-400 focus:outline-none" />
                </div>

                <div>
                  <label class="block text-xs font-semibold text-emerald-300 uppercase mb-1">Ubicación / Centro Médico</label>
                  <input type="text" v-model="searchLocation" placeholder="Ej. Buenos Aires, Centro..." class="w-full px-4 py-3 rounded-xl bg-gray-900/80 border border-gray-700 text-white placeholder-gray-500 focus:ring-2 focus:ring-emerald-400 focus:outline-none" />
                </div>

                <BaseButton buttonText="Buscar Disponibilidad" block size="lg" @click="openBooking(searchSpecialty || 'Cardiología')" />
              </div>
            </div>
          </div>

        </div>
      </div>
    </section>

    <!-- Specialties Section -->
    <section id="especialidades" class="py-20 bg-gray-50">
      <div class="max-w-screen-xl mx-auto px-4 lg:px-6">
        <div class="text-center max-w-2xl mx-auto mb-16 space-y-3">
          <span class="text-xs font-bold text-emerald-600 uppercase tracking-widest bg-emerald-100 px-3 py-1 rounded-full">Especialidades Médicas</span>
          <h2 class="text-3xl sm:text-4xl font-extrabold text-gray-900">Encuentra al especialista que necesitas</h2>
          <p class="text-gray-600">Profesionales certificados listos para atenderte con la más alta calidad humana y tecnológica.</p>
        </div>

        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
          <div 
            v-for="spec in specialties" 
            :key="spec.id"
            class="bg-white p-8 rounded-2xl shadow-sm border border-gray-100 hover:shadow-xl hover:-translate-y-1 transition-all duration-300 flex flex-col justify-between group cursor-pointer"
            @click="openBooking(spec.name)"
          >
            <div class="space-y-4">
              <div class="w-14 h-14 bg-emerald-50 rounded-2xl flex items-center justify-center text-3xl group-hover:scale-110 transition-transform">
                {{ spec.icon }}
              </div>
              <h3 class="text-xl font-bold text-gray-900">{{ spec.name }}</h3>
              <p class="text-sm text-gray-600 leading-relaxed">{{ spec.desc }}</p>
            </div>
            <div class="pt-6 mt-6 border-t border-gray-100 flex items-center justify-between">
              <span class="text-xs font-semibold text-emerald-600 bg-emerald-50 px-2.5 py-1 rounded-lg">{{ spec.doctorsCount }}</span>
              <span class="text-sm font-bold text-emerald-600 group-hover:translate-x-1 transition-transform inline-flex items-center">
                Reservar turno &rarr;
              </span>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- Benefits Section / Why Choose Us -->
    <section id="beneficios" class="py-20 bg-white">
      <div class="max-w-screen-xl mx-auto px-4 lg:px-6">
        <div class="grid grid-cols-1 lg:grid-cols-2 gap-12 items-center">
          <div class="space-y-6">
            <span class="text-xs font-bold text-emerald-600 uppercase tracking-widest bg-emerald-100 px-3 py-1 rounded-full">¿Por qué elegir MiTurno?</span>
            <h2 class="text-3xl sm:text-4xl font-extrabold text-gray-900 leading-tight">
              Una experiencia de salud moderna, rápida y sin esperas
            </h2>
            <p class="text-gray-600 leading-relaxed">
              Eliminamos las llamadas telefónicas perdidas y las largas filas en recepción. Gestiona toda tu agenda médica desde la palma de tu mano.
            </p>

            <div class="space-y-4 pt-2">
              <div class="flex items-start space-x-4">
                <div class="w-8 h-8 rounded-full bg-emerald-100 text-emerald-600 flex items-center justify-center font-bold flex-shrink-0">✓</div>
                <div>
                  <h4 class="font-bold text-gray-900">Agenda 24/7 en tiempo real</h4>
                  <p class="text-sm text-gray-600">Reserva turnos a cualquier hora del día desde cualquier dispositivo.</p>
                </div>
              </div>

              <div class="flex items-start space-x-4">
                <div class="w-8 h-8 rounded-full bg-emerald-100 text-emerald-600 flex items-center justify-center font-bold flex-shrink-0">✓</div>
                <div>
                  <h4 class="font-bold text-gray-900">Recordatorios automáticos</h4>
                  <p class="text-sm text-gray-600">Notificaciones inteligentes para que nunca olvides una cita importante.</p>
                </div>
              </div>

              <div class="flex items-start space-x-4">
                <div class="w-8 h-8 rounded-full bg-emerald-100 text-emerald-600 flex items-center justify-center font-bold flex-shrink-0">✓</div>
                <div>
                  <h4 class="font-bold text-gray-900">Historial y seguimiento médico</h4>
                  <p class="text-sm text-gray-600">Accede a tus citas pasadas y futuras de manera organizada y segura.</p>
                </div>
              </div>
            </div>
          </div>

          <!-- Decorative Card Feature -->
          <div class="bg-gradient-to-br from-emerald-600 to-teal-800 p-8 sm:p-12 rounded-3xl text-white shadow-xl relative overflow-hidden">
            <div class="absolute -bottom-10 -right-10 w-64 h-64 bg-white/10 rounded-full blur-2xl pointer-events-none"></div>
            <h3 class="text-2xl font-bold mb-4">¿Eres profesional de la salud o clínica?</h3>
            <p class="text-emerald-100 mb-8 leading-relaxed">
              Optimiza la gestión de pacientes, reduce el ausentismo y digitaliza tu consultorio con nuestras herramientas avanzadas para profesionales.
            </p>
            <BaseButton buttonText="Unirme como Profesional" variant="white" size="lg" />
          </div>
        </div>
      </div>
    </section>

    <!-- How It Works Section -->
    <section id="como-funciona" class="py-20 bg-gray-50 border-t border-b border-gray-100">
      <div class="max-w-screen-xl mx-auto px-4 lg:px-6">
        <div class="text-center max-w-2xl mx-auto mb-16 space-y-3">
          <span class="text-xs font-bold text-emerald-600 uppercase tracking-widest bg-emerald-100 px-3 py-1 rounded-full">Paso a Paso</span>
          <h2 class="text-3xl sm:text-4xl font-extrabold text-gray-900">Cómo funciona MiTurno</h2>
          <p class="text-gray-600">Reservar tu cita médica nunca fue tan sencillo y transparente.</p>
        </div>

        <div class="grid grid-cols-1 md:grid-cols-3 gap-8">
          <div v-for="step in steps" :key="step.num" class="bg-white p-8 rounded-2xl shadow-sm border border-gray-100 relative space-y-4">
            <span class="text-4xl font-black text-emerald-600/20 absolute top-6 right-6">{{ step.num }}</span>
            <div class="w-12 h-12 bg-emerald-600 text-white rounded-xl flex items-center justify-center font-bold text-lg shadow-md">
              {{ step.num }}
            </div>
            <h3 class="text-xl font-bold text-gray-900">{{ step.title }}</h3>
            <p class="text-sm text-gray-600 leading-relaxed">{{ step.desc }}</p>
          </div>
        </div>
      </div>
    </section>

    <!-- Testimonials Section -->
    <section id="testimonios" class="py-20 bg-white">
      <div class="max-w-screen-xl mx-auto px-4 lg:px-6">
        <div class="text-center max-w-2xl mx-auto mb-16 space-y-3">
          <span class="text-xs font-bold text-emerald-600 uppercase tracking-widest bg-emerald-100 px-3 py-1 rounded-full">Testimonios</span>
          <h2 class="text-3xl sm:text-4xl font-extrabold text-gray-900">Lo que dicen nuestros usuarios</h2>
          <p class="text-gray-600">Miles de pacientes confían en nosotros todos los días para cuidar su salud.</p>
        </div>

        <div class="grid grid-cols-1 md:grid-cols-3 gap-8">
          <div v-for="(t, index) in testimonials" :key="index" class="bg-gray-50 p-8 rounded-2xl border border-gray-100 space-y-4 flex flex-col justify-between">
            <div class="space-y-3">
              <div class="text-amber-400 text-lg tracking-wider">{{ t.rating }}</div>
              <p class="text-gray-700 italic text-sm leading-relaxed">"{{ t.comment }}"</p>
            </div>
            <div class="pt-4 border-t border-gray-200">
              <p class="font-bold text-gray-900 text-sm">{{ t.name }}</p>
              <p class="text-xs text-emerald-600 font-medium">{{ t.role }}</p>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- FAQ Section -->
    <section id="faq" class="py-20 bg-gray-50 border-t border-gray-100">
      <div class="max-w-screen-xl mx-auto px-4 lg:px-6">
        <div class="text-center max-w-2xl mx-auto mb-16 space-y-3">
          <span class="text-xs font-bold text-emerald-600 uppercase tracking-widest bg-emerald-100 px-3 py-1 rounded-full">Preguntas Frecuentes</span>
          <h2 class="text-3xl sm:text-4xl font-extrabold text-gray-900">Resolvemos tus dudas</h2>
          <p class="text-gray-600">Todo lo que necesitas saber sobre el uso de la plataforma MiTurno.</p>
        </div>

        <div class="max-w-3xl mx-auto space-y-4">
          <div v-for="(faq, index) in faqs" :key="index" class="bg-white p-6 rounded-2xl shadow-sm border border-gray-100">
            <h3 class="font-bold text-gray-900 text-base mb-2">{{ faq.q }}</h3>
            <p class="text-sm text-gray-600 leading-relaxed">{{ faq.a }}</p>
          </div>
        </div>
      </div>
    </section>

    <!-- CTA Banner Section -->
    <section class="py-16 bg-gradient-to-r from-gray-900 to-emerald-950 text-white text-center">
      <div class="max-w-screen-xl mx-auto px-4 lg:px-6 space-y-6">
        <h2 class="text-3xl sm:text-4xl font-black tracking-tight">Comienza a cuidar tu salud hoy mismo</h2>
        <p class="text-gray-300 max-w-xl mx-auto text-sm sm:text-base">
          Reserva tu primer turno en segundos con los mejores profesionales médicos de tu zona.
        </p>
        <div class="pt-2">
          <BaseButton buttonText="Reservar Cita Ahora" size="lg" @click="openBooking()" />
        </div>
      </div>
    </section>

    <!-- Footer -->
    <AppFooter />

    <!-- Booking Modal Component -->
    <BookingModal :isOpen="isBookingOpen" :initialSpecialty="selectedSpecialty" @close="isBookingOpen = false" />

  </div>
</template>

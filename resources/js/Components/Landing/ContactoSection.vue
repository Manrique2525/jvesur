<template>
  <section id="contacto" class="py-24 bg-[#1A2744] relative overflow-hidden">
    <!-- Decorative glow -->
    <div class="absolute bottom-0 right-0 w-96 h-96 bg-[#C9A84C]/5 rounded-full blur-3xl pointer-events-none"></div>

    <div class="relative z-10 max-w-7xl mx-auto px-6">

      <!-- Header -->
      <div class="text-center mb-16">
        <span class="inline-block text-[#C9A84C] text-xs tracking-[0.4em] uppercase font-semibold mb-3">Estamos aquí para ti</span>
        <h2 class="text-4xl md:text-5xl font-bold text-white mb-4">Contáctanos</h2>
        <div class="w-16 h-0.5 bg-[#C9A84C] mx-auto"></div>
      </div>

      <div class="grid lg:grid-cols-2 gap-12">

        <!-- Contact info -->
        <div class="space-y-8">
          <p class="text-white/60 leading-relaxed text-lg">
            ¿Tienes preguntas, necesitas oración o deseas ser parte de nuestra comunidad?
            Estamos disponibles para ti. No estás solo.
          </p>

          <div class="space-y-5">
            <div v-for="info in contactInfo" :key="info.label" class="flex items-start gap-4">
              <div class="w-11 h-11 min-w-[2.75rem] rounded-lg bg-[#C9A84C]/10 border border-[#C9A84C]/20 flex items-center justify-center">
                <span class="text-[#C9A84C] text-lg">{{ info.icon }}</span>
              </div>
              <div>
                <p class="text-[#C9A84C] text-xs tracking-widest uppercase font-semibold mb-0.5">{{ info.label }}</p>
                <a :href="info.link" class="text-white/80 hover:text-white transition-colors text-sm">{{ info.value }}</a>
              </div>
            </div>
          </div>

          <!-- Prayer request note -->
          <div class="p-5 border border-[#C9A84C]/20 rounded-xl bg-[#C9A84C]/5">
            <p class="text-[#C9A84C] font-semibold mb-2 text-sm">🙏 Petición de Oración</p>
            <p class="text-white/60 text-sm leading-relaxed">
              Si deseas que oremos por ti o tu familia, usa el formulario de contacto. 
              Nuestro equipo pastoral recibirá tu petición con confidencialidad y amor.
            </p>
          </div>
        </div>

        <!-- Form -->
        <div class="bg-white/5 border border-white/10 rounded-2xl p-8 backdrop-blur-sm">
          <h3 class="text-white font-bold text-xl mb-6">Envíanos un Mensaje</h3>
          <form @submit.prevent="submitForm" class="space-y-5">

            <div class="grid sm:grid-cols-2 gap-4">
              <div>
                <label class="text-white/60 text-xs tracking-wider uppercase block mb-1.5">Nombre</label>
                <input
                  v-model="form.name"
                  type="text"
                  placeholder="Tu nombre"
                  class="w-full bg-white/5 border border-white/10 rounded-lg px-4 py-3 text-white placeholder-white/30 text-sm focus:outline-none focus:border-[#C9A84C]/50 transition-colors"
                  required
                />
              </div>
              <div>
                <label class="text-white/60 text-xs tracking-wider uppercase block mb-1.5">Teléfono</label>
                <input
                  v-model="form.phone"
                  type="tel"
                  placeholder="+52 993 000 0000"
                  class="w-full bg-white/5 border border-white/10 rounded-lg px-4 py-3 text-white placeholder-white/30 text-sm focus:outline-none focus:border-[#C9A84C]/50 transition-colors"
                />
              </div>
            </div>

            <div>
              <label class="text-white/60 text-xs tracking-wider uppercase block mb-1.5">Correo Electrónico</label>
              <input
                v-model="form.email"
                type="email"
                placeholder="correo@ejemplo.com"
                class="w-full bg-white/5 border border-white/10 rounded-lg px-4 py-3 text-white placeholder-white/30 text-sm focus:outline-none focus:border-[#C9A84C]/50 transition-colors"
                required
              />
            </div>

            <div>
              <label class="text-white/60 text-xs tracking-wider uppercase block mb-1.5">Asunto</label>
              <select
                v-model="form.subject"
                class="w-full bg-[#1A2744] border border-white/10 rounded-lg px-4 py-3 text-white text-sm focus:outline-none focus:border-[#C9A84C]/50 transition-colors"
              >
                <option value="">Selecciona un asunto</option>
                <option value="informacion">Información general</option>
                <option value="oracion">Petición de oración</option>
                <option value="visita">Quiero visitar la iglesia</option>
                <option value="ministerio">Unirme a un ministerio</option>
                <option value="otro">Otro</option>
              </select>
            </div>

            <div>
              <label class="text-white/60 text-xs tracking-wider uppercase block mb-1.5">Mensaje</label>
              <textarea
                v-model="form.message"
                rows="4"
                placeholder="Escribe tu mensaje aquí..."
                class="w-full bg-white/5 border border-white/10 rounded-lg px-4 py-3 text-white placeholder-white/30 text-sm focus:outline-none focus:border-[#C9A84C]/50 transition-colors resize-none"
                required
              ></textarea>
            </div>

            <button
              type="submit"
              :disabled="sending"
              class="w-full py-3.5 bg-[#C9A84C] text-[#1A2744] font-bold tracking-widest text-sm uppercase rounded-lg hover:bg-[#b8943e] transition-colors disabled:opacity-60 disabled:cursor-not-allowed"
            >
              {{ sending ? 'Enviando...' : 'Enviar Mensaje' }}
            </button>

            <!-- Success message -->
            <transition name="fade">
              <div v-if="sent" class="text-center p-3 rounded-lg bg-green-500/10 border border-green-500/20">
                <p class="text-green-400 text-sm">✅ ¡Mensaje enviado! Te contactaremos pronto.</p>
              </div>
            </transition>
          </form>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref, reactive } from 'vue'

const sending = ref(false)
const sent = ref(false)

const form = reactive({
  name: '',
  phone: '',
  email: '',
  subject: '',
  message: '',
})

const contactInfo = [
  { icon: '📞', label: 'Teléfono',  value: '+52 993 000 0000', link: 'tel:+529930000000' },
  { icon: '✉️', label: 'Correo',    value: 'contacto@iglesia.com', link: 'mailto:contacto@iglesia.com' },
  { icon: '🕐', label: 'Horario de Oficina', value: 'Lunes – Viernes, 9:00 AM – 5:00 PM', link: '#' },
]

const submitForm = async () => {
  sending.value = true
  // Simula envío — conecta aquí tu endpoint de Laravel
  await new Promise(r => setTimeout(r, 1500))
  sending.value = false
  sent.value = true
  Object.assign(form, { name: '', phone: '', email: '', subject: '', message: '' })
  setTimeout(() => { sent.value = false }, 5000)
}
</script>

<style scoped>
.fade-enter-active, .fade-leave-active { transition: opacity 0.4s ease; }
.fade-enter-from, .fade-leave-to { opacity: 0; }
</style>

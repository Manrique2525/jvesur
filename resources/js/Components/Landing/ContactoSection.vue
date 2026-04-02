<template>
  <section
    id="contacto"
    class="relative py-24 bg-gradient-to-br from-[#007FE1] via-[#00BFFF] to-[#00E0A6] overflow-hidden"
  >
    <!-- Overlay -->
    <div class="absolute inset-0 bg-black/40"></div>

    <!-- Glow decorativo -->
    <div class="absolute bottom-0 right-0 w-96 h-96 bg-white/10 rounded-full blur-3xl pointer-events-none"></div>

    <div class="relative z-10 max-w-7xl mx-auto px-6">

      <!-- Header -->
      <div class="text-center mb-16">
        <span class="inline-block text-white text-xs tracking-[0.4em] uppercase font-semibold mb-3">
          Estamos aquí para ti
        </span>

        <h2 class="text-4xl md:text-5xl font-bold text-white mb-4">
          Contáctanos
        </h2>

        <div class="w-16 h-0.5 bg-[#C9A84C] mx-auto"></div>
      </div>

      <div class="grid lg:grid-cols-2 gap-12">

        <!-- Info -->
        <div class="space-y-8">
          <p class="text-white/80 leading-relaxed text-lg">
            ¿Tienes preguntas, necesitas oración o deseas ser parte de nuestra comunidad?
            Estamos disponibles para ti. No estás solo.
          </p>

          <div class="space-y-5">
            <div v-for="info in contactInfo" :key="info.label" class="flex items-start gap-4">
              <div class="w-11 h-11 min-w-[2.75rem] rounded-lg bg-white/10 border border-white/20 flex items-center justify-center">
                <span class="text-[#C9A84C] text-lg">{{ info.icon }}</span>
              </div>

              <div>
                <p class="text-white/60 text-xs tracking-widest uppercase font-semibold mb-0.5">
                  {{ info.label }}
                </p>

                <a :href="info.link" class="text-white/90 hover:text-white transition-colors text-sm">
                  {{ info.value }}
                </a>
              </div>
            </div>
          </div>

          <!-- Nota -->
          <div class="p-5 border border-white/20 rounded-xl bg-white/10 backdrop-blur-md">
            <p class="text-[#C9A84C] font-semibold mb-2 text-sm">
              🙏 Petición de Oración
            </p>

            <p class="text-white/70 text-sm leading-relaxed">
              Si deseas que oremos por ti o tu familia, usa el formulario de contacto. 
              Nuestro equipo pastoral recibirá tu petición con confidencialidad y amor.
            </p>
          </div>
        </div>

        <!-- Form -->
        <div class="bg-white/10 border border-white/20 rounded-2xl p-8 backdrop-blur-md">
          <h3 class="text-white font-bold text-xl mb-6">
            Envíanos un Mensaje
          </h3>

          <form @submit.prevent="submitForm" class="space-y-5">

            <div class="grid sm:grid-cols-2 gap-4">
              <div>
                <label class="text-white/60 text-xs uppercase mb-1.5 block">
                  Nombre
                </label>

                <input
                  v-model="form.name"
                  type="text"
                  placeholder="Tu nombre"
                  class="w-full bg-white/10 border border-white/20 rounded-lg px-4 py-3 text-white placeholder-white/40 text-sm focus:outline-none focus:border-[#C9A84C]/60"
                  required
                />
              </div>

              <div>
                <label class="text-white/60 text-xs uppercase mb-1.5 block">
                  Teléfono
                </label>

                <input
                  v-model="form.phone"
                  type="tel"
                  placeholder="+52 993 000 0000"
                  class="w-full bg-white/10 border border-white/20 rounded-lg px-4 py-3 text-white placeholder-white/40 text-sm focus:outline-none focus:border-[#C9A84C]/60"
                />
              </div>
            </div>

            <div>
              <label class="text-white/60 text-xs uppercase mb-1.5 block">
                Correo Electrónico
              </label>

              <input
                v-model="form.email"
                type="email"
                placeholder="correo@ejemplo.com"
                class="w-full bg-white/10 border border-white/20 rounded-lg px-4 py-3 text-white placeholder-white/40 text-sm focus:outline-none focus:border-[#C9A84C]/60"
                required
              />
            </div>

            <div>
              <label class="text-white/60 text-xs uppercase mb-1.5 block">
                Asunto
              </label>

              <select
                v-model="form.subject"
                class="w-full bg-white/10 border border-white/20 rounded-lg px-4 py-3 text-white text-sm focus:outline-none focus:border-[#C9A84C]/60"
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
              <label class="text-white/60 text-xs uppercase mb-1.5 block">
                Mensaje
              </label>

              <textarea
                v-model="form.message"
                rows="4"
                placeholder="Escribe tu mensaje aquí..."
                class="w-full bg-white/10 border border-white/20 rounded-lg px-4 py-3 text-white placeholder-white/40 text-sm focus:outline-none focus:border-[#C9A84C]/60 resize-none"
                required
              ></textarea>
            </div>

            <button
              type="submit"
              :disabled="sending"
              class="w-full py-3.5 bg-[#C9A84C] text-[#1E293B] font-bold tracking-widest text-sm uppercase rounded-lg hover:bg-[#b8943e] transition-all shadow-md disabled:opacity-60"
            >
              {{ sending ? 'Enviando...' : 'Enviar Mensaje' }}
            </button>

            <!-- Success -->
            <transition name="fade">
              <div v-if="sent" class="text-center p-3 rounded-lg bg-green-500/10 border border-green-500/20">
                <p class="text-green-400 text-sm">
                  ✅ ¡Mensaje enviado! Te contactaremos pronto.
                </p>
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
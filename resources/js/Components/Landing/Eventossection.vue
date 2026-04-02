<template>
  <section id="eventos" class="py-24 bg-[#FAF7F2] relative overflow-hidden">
    <!-- Decorative elements -->
    <div class="absolute top-0 right-0 w-72 h-72 bg-[#C9A84C]/5 rounded-full blur-3xl pointer-events-none -translate-y-1/2"></div>
    <div class="absolute bottom-0 left-0 w-64 h-64 bg-[#1A2744]/5 rounded-full blur-3xl pointer-events-none translate-y-1/3"></div>

    <div class="relative z-10 max-w-7xl mx-auto px-6">

      <!-- Header -->
      <div class="text-center mb-16">
        <span class="inline-block text-[#C9A84C] text-xs tracking-[0.4em] uppercase font-semibold mb-3">Calendario</span>
        <h2 class="text-4xl md:text-5xl font-bold text-[#1A2744] mb-4">Próximos Eventos</h2>
        <div class="w-16 h-0.5 bg-[#C9A84C] mx-auto"></div>
        <p class="text-gray-500 mt-6 max-w-xl mx-auto">
          Actividades y celebraciones especiales que hemos preparado para ti y tu familia. ¡Te esperamos!
        </p>
      </div>

      <!-- Evento destacado (el primero del array) -->
      <div
        v-if="featuredEvent"
        class="mb-12 rounded-2xl overflow-hidden shadow-lg border border-[#C9A84C]/10 bg-white group"
      >
        <div class="grid md:grid-cols-2">
          <!-- Imagen del evento destacado -->
          <div class="relative h-64 md:h-auto md:min-h-[360px] overflow-hidden">
            <img
              v-if="featuredEvent.image"
              :src="featuredEvent.image"
              :alt="featuredEvent.title"
              class="absolute inset-0 w-full h-full object-cover transition-transform duration-700 group-hover:scale-105"
            />
            <div
              v-else
              class="absolute inset-0 bg-[#1A2744] flex items-center justify-center"
            >
              <div class="text-center">
                <span class="text-5xl mb-3 block">{{ featuredEvent.icon || '⛪' }}</span>
                <p class="text-[#C9A84C]/40 text-xs tracking-widest uppercase">Evento Especial</p>
              </div>
              <!-- Patrón decorativo -->
              <div class="absolute inset-0 opacity-5" style="background-image: radial-gradient(circle, #C9A84C 1px, transparent 1px); background-size: 30px 30px;"></div>
            </div>

            <!-- Overlay gradiente -->
            <div class="absolute inset-0 bg-gradient-to-t from-black/40 via-transparent to-transparent md:bg-gradient-to-r md:from-transparent md:via-transparent md:to-black/10"></div>

            <!-- Botón ver imagen completa -->
            <button
              v-if="featuredEvent.image"
              @click="openLightbox(featuredEvent.image, featuredEvent.title)"
              class="absolute bottom-4 right-4 z-10 inline-flex items-center gap-1.5 px-3 py-2 rounded-lg bg-black/50 backdrop-blur-sm border border-white/20 text-white text-xs font-medium hover:bg-black/70 transition-all duration-300 opacity-0 group-hover:opacity-100"
            >
              <svg class="w-3.5 h-3.5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0zM10 7v3m0 0v3m0-3h3m-3 0H7"/>
              </svg>
              Ver imagen
            </button>

            <!-- Badge destacado -->
            <div class="absolute top-4 left-4">
              <span class="inline-flex items-center gap-1.5 px-3 py-1.5 rounded-full text-[10px] tracking-[0.15em] uppercase font-bold bg-[#C9A84C] text-[#1A2744] shadow-lg">
                <svg class="w-3 h-3" fill="currentColor" viewBox="0 0 20 20">
                  <path d="M9.049 2.927c.3-.921 1.603-.921 1.902 0l1.07 3.292a1 1 0 00.95.69h3.462c.969 0 1.371 1.24.588 1.81l-2.8 2.034a1 1 0 00-.364 1.118l1.07 3.292c.3.921-.755 1.688-1.54 1.118l-2.8-2.034a1 1 0 00-1.175 0l-2.8 2.034c-.784.57-1.838-.197-1.539-1.118l1.07-3.292a1 1 0 00-.364-1.118L2.98 8.72c-.783-.57-.38-1.81.588-1.81h3.461a1 1 0 00.951-.69l1.07-3.292z"/>
                </svg>
                Destacado
              </span>
            </div>

            <!-- Countdown (días restantes) -->
            <div
              v-if="featuredEvent.daysLeft !== null && featuredEvent.daysLeft >= 0"
              class="absolute bottom-4 left-4 md:bottom-auto md:top-4 md:left-auto md:right-4"
            >
              <div class="bg-white/95 backdrop-blur-sm rounded-xl px-4 py-2.5 text-center shadow-lg">
                <p class="text-[#1A2744] text-2xl font-black leading-none">{{ featuredEvent.daysLeft }}</p>
                <p class="text-gray-500 text-[10px] tracking-widest uppercase font-semibold mt-0.5">
                  {{ featuredEvent.daysLeft === 1 ? 'día' : 'días' }}
                </p>
              </div>
            </div>
          </div>

          <!-- Info del evento destacado -->
          <div class="p-8 md:p-10 flex flex-col justify-center">
            <!-- Categoría -->
            <span
              class="inline-block w-fit px-3 py-1 rounded-full text-[10px] tracking-[0.2em] uppercase font-bold mb-4 border"
              :style="{
                color: featuredEvent.categoryColor || '#C9A84C',
                borderColor: (featuredEvent.categoryColor || '#C9A84C') + '30',
                backgroundColor: (featuredEvent.categoryColor || '#C9A84C') + '10'
              }"
            >
              {{ featuredEvent.category }}
            </span>

            <h3 class="text-2xl md:text-3xl font-bold text-[#1A2744] mb-3 leading-tight">
              {{ featuredEvent.title }}
            </h3>

            <p class="text-gray-500 leading-relaxed mb-6">
              {{ featuredEvent.description }}
            </p>

            <!-- Detalles: Fecha, Hora, Lugar -->
            <div class="space-y-3 mb-6">
              <div class="flex items-center gap-3">
                <div class="w-9 h-9 rounded-lg bg-[#1A2744]/5 flex items-center justify-center flex-shrink-0">
                  <svg class="w-4 h-4 text-[#C9A84C]" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M8 7V3m8 4V3m-9 8h10M5 21h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v12a2 2 0 002 2z"/>
                  </svg>
                </div>
                <span class="text-[#1A2744] text-sm font-semibold">{{ featuredEvent.date }}</span>
              </div>
              <div class="flex items-center gap-3">
                <div class="w-9 h-9 rounded-lg bg-[#1A2744]/5 flex items-center justify-center flex-shrink-0">
                  <svg class="w-4 h-4 text-[#C9A84C]" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M12 8v4l3 3m6-3a9 9 0 11-18 0 9 9 0 0118 0z"/>
                  </svg>
                </div>
                <span class="text-[#1A2744] text-sm font-semibold">{{ featuredEvent.time }}</span>
              </div>
              <div v-if="featuredEvent.location" class="flex items-center gap-3">
                <div class="w-9 h-9 rounded-lg bg-[#1A2744]/5 flex items-center justify-center flex-shrink-0">
                  <svg class="w-4 h-4 text-[#C9A84C]" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M17.657 16.657L13.414 20.9a1.998 1.998 0 01-2.827 0l-4.244-4.243a8 8 0 1111.314 0z"/>
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M15 11a3 3 0 11-6 0 3 3 0 016 0z"/>
                  </svg>
                </div>
                <span class="text-gray-500 text-sm">{{ featuredEvent.location }}</span>
              </div>
            </div>

            <!-- Invitado especial -->
            <div
              v-if="featuredEvent.specialGuest"
              class="flex items-center gap-3 mb-6 p-3 rounded-xl bg-[#1A2744]/5 border border-[#1A2744]/10"
            >
              <div class="w-10 h-10 rounded-full bg-[#C9A84C]/20 border-2 border-[#C9A84C] flex items-center justify-center flex-shrink-0">
                <span class="text-[#C9A84C] font-bold text-sm">{{ featuredEvent.specialGuest.charAt(0) }}</span>
              </div>
              <div>
                <p class="text-[#C9A84C] text-[10px] tracking-widest uppercase font-semibold">Invitado Especial</p>
                <p class="text-[#1A2744] font-bold text-sm">{{ featuredEvent.specialGuest }}</p>
              </div>
            </div>

            <!-- CTA -->
            <a
              v-if="featuredEvent.actionUrl"
              :href="featuredEvent.actionUrl"
              @click.prevent="scrollTo(featuredEvent.actionUrl)"
              class="inline-flex items-center gap-2 px-7 py-3 bg-[#C9A84C] text-[#1A2744] font-bold text-sm tracking-wider uppercase rounded-xl hover:bg-[#b8943e] transition-all duration-300 hover:shadow-[0_4px_20px_rgba(201,168,76,0.3)] w-fit"
            >
              {{ featuredEvent.actionLabel || 'Más Información' }}
              <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17 8l4 4m0 0l-4 4m4-4H3"/>
              </svg>
            </a>
          </div>
        </div>
      </div>

      <!-- Grid de eventos secundarios -->
      <div
        v-if="otherEvents.length"
        class="grid sm:grid-cols-2 lg:grid-cols-3 gap-6"
      >
        <div
          v-for="event in otherEvents"
          :key="event.id"
          class="bg-white rounded-xl overflow-hidden shadow-sm border border-gray-100 hover:shadow-lg hover:border-[#C9A84C]/20 transition-all duration-400 group"
        >
          <!-- Imagen del evento -->
          <div class="relative h-48 overflow-hidden">
            <img
              v-if="event.image"
              :src="event.image"
              :alt="event.title"
              class="w-full h-full object-cover transition-transform duration-700 group-hover:scale-105"
            />
            <div
              v-else
              class="w-full h-full bg-[#1A2744] flex items-center justify-center"
            >
              <span class="text-4xl">{{ event.icon || '📅' }}</span>
              <div class="absolute inset-0 opacity-5" style="background-image: radial-gradient(circle, #C9A84C 1px, transparent 1px); background-size: 24px 24px;"></div>
            </div>

            <!-- Overlay -->
            <div class="absolute inset-0 bg-gradient-to-t from-black/30 to-transparent"></div>

            <!-- Badge de fecha -->
            <div class="absolute top-3 right-3">
              <div class="bg-white rounded-lg px-3 py-1.5 text-center shadow-md min-w-[56px]">
                <p class="text-[#C9A84C] text-xs font-black uppercase leading-none">{{ event.dateMonth }}</p>
                <p class="text-[#1A2744] text-xl font-black leading-none mt-0.5">{{ event.dateDay }}</p>
              </div>
            </div>

            <!-- Categoría -->
            <div class="absolute bottom-3 left-3">
              <span
                class="inline-block px-2.5 py-1 rounded-full text-[9px] tracking-[0.15em] uppercase font-bold backdrop-blur-sm"
                :style="{
                  color: 'white',
                  backgroundColor: (event.categoryColor || '#C9A84C') + 'CC'
                }"
              >
                {{ event.category }}
              </span>
            </div>

            <!-- Botón ver imagen -->
            <button
              v-if="event.image"
              @click.stop="openLightbox(event.image, event.title)"
              class="absolute bottom-3 right-3 z-10 w-8 h-8 rounded-lg bg-black/50 backdrop-blur-sm border border-white/20 flex items-center justify-center text-white hover:bg-black/70 transition-all duration-300 opacity-0 group-hover:opacity-100"
              title="Ver imagen completa"
            >
              <svg class="w-3.5 h-3.5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0zM10 7v3m0 0v3m0-3h3m-3 0H7"/>
              </svg>
            </button>
          </div>

          <!-- Info del evento -->
          <div class="p-5">
            <h4 class="text-[#1A2744] font-bold text-lg mb-2 leading-snug group-hover:text-[#C9A84C] transition-colors duration-300">
              {{ event.title }}
            </h4>

            <p class="text-gray-500 text-sm leading-relaxed mb-4 line-clamp-2">
              {{ event.description }}
            </p>

            <!-- Fecha y hora -->
            <div class="flex items-center gap-4 text-xs text-gray-400">
              <div class="flex items-center gap-1.5">
                <svg class="w-3.5 h-3.5 text-[#C9A84C]" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8v4l3 3m6-3a9 9 0 11-18 0 9 9 0 0118 0z"/>
                </svg>
                <span>{{ event.time }}</span>
              </div>
              <div v-if="event.location" class="flex items-center gap-1.5 truncate">
                <svg class="w-3.5 h-3.5 text-[#C9A84C] flex-shrink-0" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17.657 16.657L13.414 20.9a1.998 1.998 0 01-2.827 0l-4.244-4.243a8 8 0 1111.314 0z"/>
                </svg>
                <span class="truncate">{{ event.location }}</span>
              </div>
            </div>
          </div>
        </div>
      </div>

      <!-- Mensaje si no hay eventos -->
      <div
        v-if="!events.length"
        class="text-center py-16"
      >
        <div class="w-20 h-20 rounded-full bg-[#1A2744]/5 flex items-center justify-center mx-auto mb-4">
          <svg class="w-8 h-8 text-[#C9A84C]/50" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M8 7V3m8 4V3m-9 8h10M5 21h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v12a2 2 0 002 2z"/>
          </svg>
        </div>
        <h4 class="text-[#1A2744] font-bold text-lg mb-2">Sin eventos programados</h4>
        <p class="text-gray-400 text-sm">Pronto anunciaremos nuevas actividades. ¡Mantente atento!</p>
      </div>

      <!-- CTA final -->
      <div
        v-if="events.length"
        class="mt-12 text-center"
      >
        <div class="inline-flex items-center gap-3 px-6 py-3 rounded-full bg-[#1A2744]/5 border border-[#1A2744]/10">
          <svg class="w-4 h-4 text-[#C9A84C]" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 17h5l-1.405-1.405A2.032 2.032 0 0118 14.158V11a6.002 6.002 0 00-4-5.659V5a2 2 0 10-4 0v.341C7.67 6.165 6 8.388 6 11v3.159c0 .538-.214 1.055-.595 1.436L4 17h5m6 0v1a3 3 0 11-6 0v-1m6 0H9"/>
          </svg>
          <span class="text-[#1A2744] text-sm font-medium">Síguenos en redes sociales para enterarte de más eventos</span>
        </div>
      </div>

    </div>
  </section>

  <!-- Lightbox para ver imagen completa -->
  <Teleport to="body">
    <Transition name="lightbox">
      <div
        v-if="lightbox.open"
        class="fixed inset-0 z-[9999] flex items-center justify-center p-4"
        @click.self="closeLightbox"
      >
        <!-- Backdrop -->
        <div class="absolute inset-0 bg-black/85 backdrop-blur-sm"></div>

        <!-- Imagen -->
        <div class="relative z-10 max-w-lg w-full max-h-[90vh]">
          <!-- Cerrar -->
          <button
            @click="closeLightbox"
            class="absolute -top-3 -right-3 z-20 w-10 h-10 rounded-full bg-[#1A2744] border-2 border-[#C9A84C]/50 flex items-center justify-center text-white hover:bg-[#C9A84C] hover:text-[#1A2744] transition-all duration-300"
          >
            <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"/>
            </svg>
          </button>

          <!-- Imagen con rounded -->
          <img
            :src="lightbox.image"
            :alt="lightbox.alt"
            class="w-full h-auto max-h-[85vh] object-contain rounded-2xl shadow-2xl"
          />

          <!-- Título debajo de la imagen -->
          <p
            v-if="lightbox.alt"
            class="text-center text-white/60 text-sm mt-3 font-medium"
          >
            {{ lightbox.alt }}
          </p>
        </div>
      </div>
    </Transition>
  </Teleport>
</template>

<script setup>
import { ref, reactive, computed, onMounted, onUnmounted } from 'vue'

const props = defineProps({
  /**
   * Array de eventos del backend.
   * Si es null, usa los datos estáticos.
   * Estructura de cada evento:
   * {
   *   id, title, description, category, categoryColor,
   *   date, dateDay, dateMonth, time, location,
   *   image, icon, specialGuest,
   *   featured, daysLeft,
   *   actionLabel, actionUrl
   * }
   */
  eventsList: {
    type: Array,
    default: null
  }
})

// ─────────────────────────────────────────
// DATOS ESTÁTICOS DE EVENTOS (FÁCIL DE EDITAR)
// Cambia, agrega o elimina eventos aquí.
// Cuando conectes el backend, pasa los datos via prop :events-list="backendEvents"
// ─────────────────────────────────────────
const defaultEvents = [
  {
    id: 1,
    featured: true,
    title: 'Culto de Aniversario',
    description: 'Ven y celebra con nosotros nuestro 10mo. Aniversario donde tendremos un culto especial, llenos de alegría y gozo. Una celebración inolvidable con toda la familia de la iglesia.',
    category: 'Celebración',
    categoryColor: '#C9A84C',
    date: 'Domingo, 05 de Abril 2026',
    dateDay: '05',
    dateMonth: 'ABR',
    time: '10:00 AM',
    location: 'Templo Principal',
    image:  '/images/eventos/culto-aniversario.jpg',
    icon: '🎉',
    specialGuest: 'Pastor Constantino Varas',
    daysLeft: 4,   // null para no mostrar countdown
    actionLabel: 'Quiero Asistir',
    actionUrl: '#contacto',
  },
  {
    id: 2,
    featured: false,
    title: 'Noche de Alabanza',
    description: 'Una noche especial dedicada a la adoración y la música. Ven a disfrutar de un tiempo de alabanza con todos los ministerios musicales de la iglesia.',
    category: 'Alabanza',
    categoryColor: '#6366F1',
    date: 'Sábado, 12 de Abril 2026',
    dateDay: '12',
    dateMonth: 'ABR',
    time: '7:00 PM',
    location: 'Templo Principal',
    image: '/images/image3.jpg',
    icon: '🎵',
    specialGuest: null,
    daysLeft: null,
    actionLabel: null,
    actionUrl: null,
  },
  {
    id: 3,
    featured: false,
    title: 'Campamento Jóvenes',
    description: 'Tres días de convivencia, enseñanza y crecimiento espiritual para nuestros jóvenes. Un tiempo para conectar con Dios y con otros jóvenes.',
    category: 'Jóvenes',
    categoryColor: '#F59E0B',
    date: '18-20 de Abril 2026',
    dateDay: '18',
    dateMonth: 'ABR',
    time: 'Viernes 4:00 PM',
    location: 'Centro de Retiros',
    image: '/images/image1.jpg',
    icon: '🔥',
    specialGuest: null,
    daysLeft: null,
    actionLabel: null,
    actionUrl: null,
  },
  {
    id: 4,
    featured: false,
    title: 'Escuela Bíblica de Verano',
    description: 'Actividades, juegos y enseñanza bíblica para los más pequeños durante las vacaciones de verano. ¡Inscripciones abiertas!',
    category: 'Niños',
    categoryColor: '#10B981',
    date: 'Julio 2026',
    dateDay: '07',
    dateMonth: 'JUL',
    time: '9:00 AM - 1:00 PM',
    location: 'Salón de Niños',
    image: '/images/image2.jpg',
    icon: '👶',
    specialGuest: null,
    daysLeft: null,
    actionLabel: null,
    actionUrl: null,
  },
]

// Eventos: usa props si existe, si no los estáticos
const events = computed(() => props.eventsList ?? defaultEvents)

// Evento destacado (el primero con featured: true, o el primero del array)
const featuredEvent = computed(() => {
  return events.value.find(e => e.featured) || events.value[0] || null
})

// Eventos restantes (sin el destacado)
const otherEvents = computed(() => {
  if (!featuredEvent.value) return events.value
  return events.value.filter(e => e.id !== featuredEvent.value.id)
})

// Scroll suave
const scrollTo = (href) => {
  if (!href) return
  const el = document.querySelector(href)
  if (el) el.scrollIntoView({ behavior: 'smooth' })
}

// ── Lightbox ──
const lightbox = reactive({
  open: false,
  image: '',
  alt: ''
})

const openLightbox = (image, alt) => {
  lightbox.image = image
  lightbox.alt = alt || ''
  lightbox.open = true
  document.body.style.overflow = 'hidden'
}

const closeLightbox = () => {
  lightbox.open = false
  document.body.style.overflow = ''
}

// Cerrar con Escape
const onKeydown = (e) => {
  if (e.key === 'Escape' && lightbox.open) closeLightbox()
}

onMounted(() => document.addEventListener('keydown', onKeydown))
onUnmounted(() => {
  document.removeEventListener('keydown', onKeydown)
  document.body.style.overflow = ''
})
</script>

<style scoped>
.line-clamp-2 {
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

/* Lightbox transitions */
.lightbox-enter-active { transition: opacity 0.3s ease; }
.lightbox-leave-active { transition: opacity 0.25s ease; }
.lightbox-enter-from,
.lightbox-leave-to { opacity: 0; }
</style>

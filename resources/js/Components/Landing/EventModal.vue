<template>
  <Teleport to="body">
    <Transition name="modal-backdrop">
      <div
        v-if="isVisible"
        class="fixed inset-0 z-[9999] flex items-center justify-center p-4"
        @click.self="handleClose"
      >
        <!-- Backdrop -->
        <div class="absolute inset-0 bg-black/70 backdrop-blur-sm"></div>

        <!-- Modal -->
        <Transition name="modal-content" appear>
          <div
            v-if="showContent"
            class="relative w-full max-w-lg rounded-2xl overflow-hidden shadow-2xl"
            :style="{ maxHeight: 'calc(100vh - 2rem)' }"
          >
            <!-- Close button -->
            <button
              @click="handleClose"
              class="absolute top-3 right-3 z-30 w-9 h-9 rounded-full bg-black/50 backdrop-blur-sm border border-white/20 flex items-center justify-center text-white/80 hover:text-white hover:bg-black/70 transition-all duration-300 hover:rotate-90"
              aria-label="Cerrar"
            >
              <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"/>
              </svg>
            </button>

            <!-- Scrollable content -->
            <div class="overflow-y-auto" :style="{ maxHeight: 'calc(100vh - 2rem)' }">
              <!-- Header con imagen/gradiente -->
              <div class="relative h-52 sm:h-60 overflow-hidden">
                <!-- Imagen de fondo del evento -->
                <div
                  v-if="eventData.headerImage"
                  class="absolute inset-0 bg-cover bg-center"
                  :style="{ backgroundImage: `url(${eventData.headerImage})` }"
                ></div>
                <!-- Fallback gradiente si no hay imagen -->
                <div
                  v-else
                  class="absolute inset-0"
                  :style="{
                    background: `linear-gradient(135deg, ${eventData.colors.primary} 0%, ${eventData.colors.primaryDark || '#0f1a2e'} 100%)`
                  }"
                >
                  <!-- Patrón decorativo -->
                  <div class="absolute inset-0 opacity-10" style="background-image: radial-gradient(circle, rgba(255,255,255,0.3) 1px, transparent 1px); background-size: 24px 24px;"></div>
                  <!-- Cruz decorativa -->
                  <svg class="absolute right-6 top-6 opacity-15" width="80" height="120" viewBox="0 0 60 90" fill="none">
                    <rect x="22" y="0" width="16" height="90" rx="3" :fill="eventData.colors.accent"/>
                    <rect x="0" y="22" width="60" height="16" rx="3" :fill="eventData.colors.accent"/>
                  </svg>
                </div>

                <!-- Overlay gradiente inferior -->
                <div
                  class="absolute inset-0"
                  :style="{
                    background: `linear-gradient(to top, ${eventData.colors.primary} 0%, transparent 60%)`
                  }"
                ></div>

                <!-- Badge del tipo de evento -->
                <div class="absolute top-4 left-4 z-10">
                  <span
                    class="inline-flex items-center gap-1.5 px-3 py-1 rounded-full text-[10px] tracking-[0.2em] uppercase font-bold backdrop-blur-sm border"
                    :style="{
                      backgroundColor: `${eventData.colors.accent}20`,
                      borderColor: `${eventData.colors.accent}50`,
                      color: eventData.colors.accent
                    }"
                  >
                    <span
                      class="w-1.5 h-1.5 rounded-full animate-pulse"
                      :style="{ backgroundColor: eventData.colors.accent }"
                    ></span>
                    {{ eventData.badge }}
                  </span>
                </div>

                <!-- Título principal sobre la imagen -->
                <div class="absolute bottom-4 left-5 right-5 z-10">
                  <h2
                    class="text-2xl sm:text-3xl font-extrabold leading-tight"
                    :style="{ color: eventData.colors.accent }"
                  >
                    {{ eventData.title }}
                  </h2>
                  <p
                    v-if="eventData.subtitle"
                    class="text-sm mt-1 font-medium opacity-80"
                    style="color: white;"
                  >
                    {{ eventData.subtitle }}
                  </p>
                </div>
              </div>

              <!-- Body -->
              <div
                class="px-5 pt-5 pb-6"
                :style="{ backgroundColor: eventData.colors.primary }"
              >
                <!-- Descripción -->
                <p class="text-white/70 text-sm leading-relaxed mb-5">
                  {{ eventData.description }}
                </p>

                <!-- Invitado especial (si existe) -->
                <div
                  v-if="eventData.specialGuest"
                  class="flex items-start gap-3 mb-5 p-3.5 rounded-xl border"
                  :style="{
                    borderColor: `${eventData.colors.accent}25`,
                    backgroundColor: `${eventData.colors.accent}08`
                  }"
                >
                  <!-- Avatar del invitado -->
                  <div
                    class="w-12 h-12 rounded-full flex-shrink-0 overflow-hidden border-2"
                    :style="{ borderColor: eventData.colors.accent }"
                  >
                    <img
                      v-if="eventData.specialGuest.image"
                      :src="eventData.specialGuest.image"
                      :alt="eventData.specialGuest.name"
                      class="w-full h-full object-cover"
                    />
                    <div
                      v-else
                      class="w-full h-full flex items-center justify-center text-lg"
                      :style="{ backgroundColor: `${eventData.colors.accent}20`, color: eventData.colors.accent }"
                    >
                      {{ eventData.specialGuest.name.charAt(0) }}
                    </div>
                  </div>
                  <div class="min-w-0">
                    <p
                      class="text-xs tracking-widest uppercase font-semibold mb-0.5"
                      :style="{ color: eventData.colors.accent }"
                    >
                      Invitado Especial
                    </p>
                    <p class="text-white font-bold text-sm">{{ eventData.specialGuest.name }}</p>
                    <p class="text-white/50 text-xs">{{ eventData.specialGuest.role }}</p>
                  </div>
                </div>

                <!-- Fecha, hora y ubicación -->
                <div class="space-y-2.5 mb-5">
                  <!-- Fecha -->
                  <div class="flex items-center gap-3">
                    <div
                      class="w-10 h-10 rounded-lg flex items-center justify-center flex-shrink-0"
                      :style="{
                        backgroundColor: `${eventData.colors.accent}15`,
                        border: `1px solid ${eventData.colors.accent}30`
                      }"
                    >
                      <svg class="w-4.5 h-4.5" :style="{ color: eventData.colors.accent }" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M8 7V3m8 4V3m-9 8h10M5 21h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v12a2 2 0 002 2z"/>
                      </svg>
                    </div>
                    <div>
                      <p class="text-white font-bold text-sm">{{ eventData.date }}</p>
                      <p class="text-white/40 text-xs">{{ eventData.dateExtra || '' }}</p>
                    </div>
                  </div>

                  <!-- Hora -->
                  <div class="flex items-center gap-3">
                    <div
                      class="w-10 h-10 rounded-lg flex items-center justify-center flex-shrink-0"
                      :style="{
                        backgroundColor: `${eventData.colors.accent}15`,
                        border: `1px solid ${eventData.colors.accent}30`
                      }"
                    >
                      <svg class="w-4.5 h-4.5" :style="{ color: eventData.colors.accent }" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M12 8v4l3 3m6-3a9 9 0 11-18 0 9 9 0 0118 0z"/>
                      </svg>
                    </div>
                    <div>
                      <p class="text-white font-bold text-sm">{{ eventData.time }}</p>
                      <p class="text-white/40 text-xs">{{ eventData.timeExtra || '' }}</p>
                    </div>
                  </div>

                  <!-- Ubicación -->
                  <div v-if="eventData.location" class="flex items-start gap-3">
                    <div
                      class="w-10 h-10 rounded-lg flex items-center justify-center flex-shrink-0"
                      :style="{
                        backgroundColor: `${eventData.colors.accent}15`,
                        border: `1px solid ${eventData.colors.accent}30`
                      }"
                    >
                      <svg class="w-4.5 h-4.5" :style="{ color: eventData.colors.accent }" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M17.657 16.657L13.414 20.9a1.998 1.998 0 01-2.827 0l-4.244-4.243a8 8 0 1111.314 0z"/>
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M15 11a3 3 0 11-6 0 3 3 0 016 0z"/>
                      </svg>
                    </div>
                    <div>
                      <p class="text-white font-bold text-sm">{{ eventData.location.name || 'Ubicación' }}</p>
                      <p class="text-white/40 text-xs leading-relaxed">{{ eventData.location.address }}</p>
                    </div>
                  </div>
                </div>

                <!-- Botones de acción -->
                <div class="flex flex-col sm:flex-row gap-2.5">
                  <button
                    v-if="eventData.primaryAction"
                    @click="handlePrimaryAction"
                    class="flex-1 py-3 rounded-xl font-bold text-sm tracking-wider uppercase transition-all duration-300 hover:shadow-lg"
                    :style="{
                      backgroundColor: eventData.colors.accent,
                      color: eventData.colors.primary,
                      boxShadow: `0 4px 20px ${eventData.colors.accent}40`
                    }"
                  >
                    {{ eventData.primaryAction.label }}
                  </button>
                  <button
                    v-if="eventData.secondaryAction"
                    @click="handleSecondaryAction"
                    class="flex-1 py-3 rounded-xl font-bold text-sm tracking-wider uppercase transition-all duration-300 border"
                    :style="{
                      borderColor: `${eventData.colors.accent}40`,
                      color: eventData.colors.accent,
                      backgroundColor: 'transparent'
                    }"
                  >
                    {{ eventData.secondaryAction.label }}
                  </button>
                </div>

                <!-- Checkbox "No mostrar de nuevo" -->
                <label
                  v-if="eventData.showDontShowAgain !== false"
                  class="flex items-center gap-2 mt-4 cursor-pointer group"
                >
                  <input
                    type="checkbox"
                    v-model="dontShowAgain"
                    class="w-3.5 h-3.5 rounded border-white/30 bg-transparent accent-current"
                    :style="{ accentColor: eventData.colors.accent }"
                  />
                  <span class="text-white/40 text-xs group-hover:text-white/60 transition-colors">
                    No mostrar de nuevo
                  </span>
                </label>
              </div>
            </div>
          </div>
        </Transition>
      </div>
    </Transition>
  </Teleport>
</template>

<script setup>
import { ref, onMounted, watch, computed } from 'vue'

const props = defineProps({
  /**
   * Datos del evento a mostrar.
   * Si se pasa null, usa los datos estáticos por defecto.
   * Estructura esperada del backend:
   * {
   *   id, title, subtitle, badge, description,
   *   date, dateExtra, time, timeExtra,
   *   headerImage, specialGuest: { name, role, image },
   *   location: { name, address },
   *   colors: { primary, accent, primaryDark },
   *   primaryAction: { label, url },
   *   secondaryAction: { label, url },
   *   showDontShowAgain, storageKey, delay
   * }
   */
  event: {
    type: Object,
    default: null
  },
  /** Mostrar el modal (control externo) */
  modelValue: {
    type: Boolean,
    default: undefined
  },
  /** Abrir automáticamente al montar el componente */
  autoOpen: {
    type: Boolean,
    default: true
  },
  /** Delay en ms antes de abrir automáticamente */
  delay: {
    type: Number,
    default: null // Si es null, usa eventData.delay
  }
})

const emit = defineEmits(['update:modelValue', 'close', 'primary-action', 'secondary-action'])

// ─────────────────────────────────────────
// DATOS ESTÁTICOS DEL EVENTO (FÁCIL DE EDITAR)
// Cambia estos valores para actualizar el evento.
// Cuando conectes el backend, pasa los datos via prop :event="backendData"
// ─────────────────────────────────────────
const defaultEventData = {
  // Identificador único (para localStorage)
  id: 'culto-aniversario-2026',

  // Textos principales
  badge: 'Evento Especial',
  title: 'Culto de Aniversario',
  subtitle: '10mo Aniversario de nuestra iglesia',
  description: 'Ven y celebra con nosotros nuestro 10mo. Aniversario donde tendremos un culto especial, llenos de alegría y gozo. Una celebración que no te puedes perder.',

  // Fecha y hora
  date: 'Domingo, 05 de Abril 2026',
  dateExtra: 'Próximo domingo',
  time: '10:00 AM',
  timeExtra: 'Puntualidad',

  // Imagen de cabecera (null = usa gradiente con patrón)
  // Puedes poner una URL como: '/images/eventos/aniversario.jpg'
  headerImage: null,

  // Invitado especial (null = no mostrar sección)
  specialGuest: {
    name: 'Pastor Constantino Varas',
    role: 'Presidente de la CNBM',
    image: null // URL de imagen o null para mostrar inicial
  },

  // Ubicación (null = no mostrar)
  location: {
    name: 'Iglesia Jesucristo es la Vida Eterna Sur',
    address: 'Carret. a Teapa a 100 mts. del Periférico Carlos Pellicer Cámara, frente a CITY CLUB, Col. Plutarco Elías Calles'
  },

  // Colores del modal (personaliza por evento)
  colors: {
    primary: '#1A2744',      // Fondo principal
    primaryDark: '#0f1a2e',  // Gradiente oscuro
    accent: '#C9A84C'        // Color dorado / acento
  },

  // Botones de acción
  primaryAction: {
    label: '¡Quiero Asistir!',
    url: '#contacto'
  },
  secondaryAction: {
    label: 'Más Información',
    url: '#servicios'
  },

  // Configuración
  showDontShowAgain: true,
  storageKey: 'event-modal-dismissed', // Clave para localStorage
  delay: 1500 // ms antes de mostrar el modal
}

// Datos computados: usa props.event si existe, si no los estáticos
const eventData = computed(() => {
  if (props.event) {
    // Merge profundo para que no falten campos
    return {
      ...defaultEventData,
      ...props.event,
      colors: { ...defaultEventData.colors, ...(props.event.colors || {}) },
      specialGuest: props.event.specialGuest === null
        ? null
        : props.event.specialGuest
          ? { ...defaultEventData.specialGuest, ...props.event.specialGuest }
          : defaultEventData.specialGuest,
      location: props.event.location === null
        ? null
        : props.event.location
          ? { ...defaultEventData.location, ...props.event.location }
          : defaultEventData.location,
      primaryAction: props.event.primaryAction === null
        ? null
        : props.event.primaryAction
          ? { ...defaultEventData.primaryAction, ...props.event.primaryAction }
          : defaultEventData.primaryAction,
      secondaryAction: props.event.secondaryAction === null
        ? null
        : props.event.secondaryAction
          ? { ...defaultEventData.secondaryAction, ...props.event.secondaryAction }
          : defaultEventData.secondaryAction,
    }
  }
  return defaultEventData
})

// Estado del modal
const isVisible = ref(false)
const showContent = ref(false)
const dontShowAgain = ref(false)

// Controlar visibilidad
const open = () => {
  isVisible.value = true
  setTimeout(() => { showContent.value = true }, 50)
}

const handleClose = () => {
  showContent.value = false
  setTimeout(() => {
    isVisible.value = false
    emit('update:modelValue', false)
    emit('close')
    // Guardar preferencia
    if (dontShowAgain.value && eventData.value.storageKey) {
      try {
        localStorage.setItem(
          `${eventData.value.storageKey}-${eventData.value.id}`,
          Date.now().toString()
        )
      } catch (e) { /* localStorage no disponible */ }
    }
  }, 300)
}

// Control externo con v-model
if (props.modelValue !== undefined) {
  watch(() => props.modelValue, (val) => {
    if (val) open()
    else handleClose()
  })
}

// Acciones
const handlePrimaryAction = () => {
  emit('primary-action', eventData.value.primaryAction)
  if (eventData.value.primaryAction?.url) {
    const el = document.querySelector(eventData.value.primaryAction.url)
    if (el) {
      handleClose()
      setTimeout(() => el.scrollIntoView({ behavior: 'smooth' }), 350)
    }
  }
}

const handleSecondaryAction = () => {
  emit('secondary-action', eventData.value.secondaryAction)
  if (eventData.value.secondaryAction?.url) {
    const el = document.querySelector(eventData.value.secondaryAction.url)
    if (el) {
      handleClose()
      setTimeout(() => el.scrollIntoView({ behavior: 'smooth' }), 350)
    }
  }
}

// Cerrar con Escape
const onKeydown = (e) => {
  if (e.key === 'Escape' && isVisible.value) handleClose()
}

// Auto-open al montar
onMounted(() => {
  document.addEventListener('keydown', onKeydown)

  if (props.autoOpen && props.modelValue === undefined) {
    // Verificar si ya fue descartado
    const dismissed = (() => {
      try {
        return localStorage.getItem(`${eventData.value.storageKey}-${eventData.value.id}`)
      } catch { return null }
    })()

    if (!dismissed) {
      const delayMs = props.delay ?? eventData.value.delay ?? 1500
      setTimeout(() => open(), delayMs)
    }
  }
})

// Exponer métodos para control externo
defineExpose({ open, close: handleClose })
</script>

<style scoped>
/* Backdrop */
.modal-backdrop-enter-active { transition: opacity 0.4s ease; }
.modal-backdrop-leave-active { transition: opacity 0.3s ease; }
.modal-backdrop-enter-from,
.modal-backdrop-leave-to { opacity: 0; }

/* Content */
.modal-content-enter-active {
  transition: all 0.5s cubic-bezier(0.16, 1, 0.3, 1);
}
.modal-content-leave-active {
  transition: all 0.3s ease-in;
}
.modal-content-enter-from {
  opacity: 0;
  transform: translateY(40px) scale(0.95);
}
.modal-content-leave-to {
  opacity: 0;
  transform: translateY(20px) scale(0.97);
}

/* Scrollbar personalizada */
.overflow-y-auto::-webkit-scrollbar {
  width: 4px;
}
.overflow-y-auto::-webkit-scrollbar-track {
  background: transparent;
}
.overflow-y-auto::-webkit-scrollbar-thumb {
  background: rgba(201, 168, 76, 0.3);
  border-radius: 999px;
}
</style>

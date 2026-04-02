<template>
  <Teleport to="body">
    <Transition name="modal-backdrop">
      <div
        v-if="isVisible"
        class="fixed inset-0 z-[9999] flex items-center justify-center p-4"
        @click.self="handleClose"
      >
        <!-- Backdrop -->
        <div class="absolute inset-0 bg-black/80 backdrop-blur-sm"></div>

        <!-- Modal -->
        <Transition name="modal-content" appear>
          <div
            v-if="showContent"
            class="relative w-full rounded-2xl overflow-hidden shadow-2xl"
            :style="{
              maxWidth: modalData.maxWidth || '480px',
              maxHeight: 'calc(100vh - 2rem)'
            }"
          >
            <!-- Close button -->
            <button
              @click="handleClose"
              class="absolute top-3 right-3 z-30 w-9 h-9 rounded-full flex items-center justify-center transition-all duration-300 hover:rotate-90"
              :class="modalData.closeButtonStyle === 'light'
                ? 'bg-white/80 text-gray-800 hover:bg-white shadow-lg'
                : 'bg-black/50 backdrop-blur-sm border border-white/20 text-white/80 hover:text-white hover:bg-black/70'"
              aria-label="Cerrar"
            >
              <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"/>
              </svg>
            </button>

            <!-- Scrollable wrapper -->
            <div class="overflow-y-auto" :style="{ maxHeight: 'calc(100vh - 2rem)' }">

              <!-- Imagen principal -->
              <div class="relative">
                <img
                  :src="modalData.image"
                  :alt="modalData.alt || 'Evento'"
                  class="w-full h-auto block"
                  :class="{ 'cursor-pointer': modalData.linkUrl }"
                  @click="handleImageClick"
                  @load="imageLoaded = true"
                  @error="imageError = true"
                />

                <!-- Placeholder mientras carga -->
                <div
                  v-if="!imageLoaded && !imageError"
                  class="absolute inset-0 flex items-center justify-center bg-gray-900"
                >
                  <div class="flex flex-col items-center gap-3">
                    <svg class="w-8 h-8 text-gray-600 animate-pulse" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                      <path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M4 16l4.586-4.586a2 2 0 012.828 0L16 16m-2-2l1.586-1.586a2 2 0 012.828 0L20 14m-6-6h.01M6 20h12a2 2 0 002-2V6a2 2 0 00-2-2H6a2 2 0 00-2 2v12a2 2 0 002 2z"/>
                    </svg>
                    <span class="text-gray-500 text-xs">Cargando imagen...</span>
                  </div>
                </div>

                <!-- Error al cargar -->
                <div
                  v-if="imageError"
                  class="flex items-center justify-center bg-gray-900 py-20"
                >
                  <div class="flex flex-col items-center gap-3 text-center px-6">
                    <svg class="w-10 h-10 text-red-400/60" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                      <path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M12 9v2m0 4h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z"/>
                    </svg>
                    <p class="text-gray-400 text-sm">No se pudo cargar la imagen</p>
                    <p class="text-gray-600 text-xs break-all">{{ modalData.image }}</p>
                  </div>
                </div>
              </div>

              <!-- Footer con botón de acción (opcional) -->
              <div
                v-if="modalData.actionLabel"
                class="px-5 py-4"
                :style="{ backgroundColor: modalData.footerBg || '#1A2744' }"
              >
                <button
                  @click="handleAction"
                  class="w-full py-3 rounded-xl font-bold text-sm tracking-wider uppercase transition-all duration-300 hover:shadow-lg"
                  :style="{
                    backgroundColor: modalData.actionColor || '#C9A84C',
                    color: modalData.actionTextColor || '#1A2744',
                    boxShadow: `0 4px 20px ${modalData.actionColor || '#C9A84C'}40`
                  }"
                >
                  {{ modalData.actionLabel }}
                </button>

                <!-- Texto pequeño debajo del botón -->
                <p
                  v-if="modalData.actionSubtext"
                  class="text-center text-xs mt-2 opacity-50"
                  :style="{ color: modalData.actionColor || '#C9A84C' }"
                >
                  {{ modalData.actionSubtext }}
                </p>
              </div>
            </div>

            <!-- Checkbox "No mostrar de nuevo" -->
            <div
              v-if="modalData.showDontShowAgain !== false"
              class="absolute bottom-0 left-0 right-0 z-20"
              :class="modalData.actionLabel ? '' : 'px-4 pb-3'"
            >
              <label
                v-if="!modalData.actionLabel"
                class="flex items-center gap-2 cursor-pointer bg-black/60 backdrop-blur-sm rounded-lg px-3 py-2 w-fit"
              >
                <input
                  type="checkbox"
                  v-model="dontShowAgain"
                  class="w-3.5 h-3.5 rounded"
                />
                <span class="text-white/70 text-xs">No mostrar de nuevo</span>
              </label>
            </div>
          </div>
        </Transition>
      </div>
    </Transition>
  </Teleport>
</template>

<script setup>
import { ref, computed, onMounted, watch } from 'vue'

const props = defineProps({
  /**
   * Datos del modal de imagen.
   * Si es null, usa los datos estáticos por defecto.
   * Estructura esperada del backend:
   * {
   *   id, image, alt, maxWidth,
   *   linkUrl, linkTarget,
   *   actionLabel, actionUrl, actionColor, actionTextColor, actionSubtext,
   *   footerBg, closeButtonStyle,
   *   showDontShowAgain, storageKey, delay
   * }
   */
  modal: {
    type: Object,
    default: null
  },
  /** Control externo con v-model */
  modelValue: {
    type: Boolean,
    default: undefined
  },
  /** Abrir automáticamente al montar */
  autoOpen: {
    type: Boolean,
    default: true
  },
  /** Delay en ms antes de abrir */
  delay: {
    type: Number,
    default: null
  }
})

const emit = defineEmits(['update:modelValue', 'close', 'action', 'image-click'])

// ─────────────────────────────────────────
// DATOS ESTÁTICOS (FÁCIL DE CAMBIAR)
// Cambia estos valores para actualizar la imagen.
// Cuando conectes el backend, pasa los datos via prop :modal="backendData"
// ─────────────────────────────────────────
const defaultModalData = {
  // Identificador único (para localStorage)
  id: 'culto-aniversario-2026',

  // Imagen del evento (ruta relativa o URL completa)
  image: '/images/eventos/culto-aniversario.jpg',
  alt: 'Culto de Aniversario - Domingo 05 de Abril, 10:00 AM',

  // Tamaño máximo del modal (ajusta según la imagen)
  maxWidth: '480px',

  // Si la imagen debe ser clickeable (null = no)
  linkUrl: null,       // Ejemplo: 'https://facebook.com/evento'
  linkTarget: '_blank', // '_blank' | '_self'

  // Botón de acción debajo de la imagen (null = no mostrar)
  actionLabel: '¡Quiero Asistir!',
  actionUrl: '#contacto',
  actionColor: '#C9A84C',
  actionTextColor: '#1A2744',
  actionSubtext: 'Domingo 05 de Abril — 10:00 AM',
  footerBg: '#1A2744',

  // Estilo del botón de cerrar: 'dark' (por defecto) o 'light'
  closeButtonStyle: 'dark',

  // Configuración
  showDontShowAgain: true,
  storageKey: 'event-image-modal-dismissed',
  delay: 1000
}

// Merge con props
const modalData = computed(() => {
  if (props.modal) {
    return { ...defaultModalData, ...props.modal }
  }
  return defaultModalData
})

// Estado
const isVisible = ref(false)
const showContent = ref(false)
const dontShowAgain = ref(false)
const imageLoaded = ref(false)
const imageError = ref(false)

// Controlar visibilidad
const open = () => {
  imageLoaded.value = false
  imageError.value = false
  isVisible.value = true
  setTimeout(() => { showContent.value = true }, 500)
}

const handleClose = () => {
  showContent.value = false
  setTimeout(() => {
    isVisible.value = false
    emit('update:modelValue', false)
    emit('close')
    if (dontShowAgain.value && modalData.value.storageKey) {
      try {
        localStorage.setItem(
          `${modalData.value.storageKey}-${modalData.value.id}`,
          Date.now().toString()
        )
      } catch (e) { /* localStorage no disponible */ }
    }
  }, 300)
}

// Acciones
const handleImageClick = () => {
  emit('image-click', modalData.value)
  if (modalData.value.linkUrl) {
    window.open(modalData.value.linkUrl, modalData.value.linkTarget || '_blank')
  }
}

const handleAction = () => {
  emit('action', modalData.value)
  if (modalData.value.actionUrl) {
    // Si es un anchor (#seccion), hacer scroll
    if (modalData.value.actionUrl.startsWith('#')) {
      const el = document.querySelector(modalData.value.actionUrl)
      if (el) {
        handleClose()
        setTimeout(() => el.scrollIntoView({ behavior: 'smooth' }), 350)
        return
      }
    }
    // Si es URL externa, abrir
    window.open(modalData.value.actionUrl, modalData.value.linkTarget || '_self')
  }
}

// Cerrar con Escape
const onKeydown = (e) => {
  if (e.key === 'Escape' && isVisible.value) handleClose()
}

// Control externo con v-model
if (props.modelValue !== undefined) {
  watch(() => props.modelValue, (val) => {
    if (val) open()
    else handleClose()
  })
}

// Auto-open
onMounted(() => {
  document.addEventListener('keydown', onKeydown)

  if (props.autoOpen && props.modelValue === undefined) {
    const dismissed = (() => {
      try {
        return localStorage.getItem(`${modalData.value.storageKey}-${modalData.value.id}`)
      } catch { return null }
    })()

    if (!dismissed) {
      const delayMs = props.delay ?? modalData.value.delay ?? 1500
      setTimeout(() => open(), delayMs)
    }
  }
})

defineExpose({ open, close: handleClose })
</script>

<style scoped>
.modal-backdrop-enter-active { transition: opacity 0.4s ease; }
.modal-backdrop-leave-active { transition: opacity 0.3s ease; }
.modal-backdrop-enter-from,
.modal-backdrop-leave-to { opacity: 0; }

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

.overflow-y-auto::-webkit-scrollbar { width: 4px; }
.overflow-y-auto::-webkit-scrollbar-track { background: transparent; }
.overflow-y-auto::-webkit-scrollbar-thumb {
  background: rgba(255, 255, 255, 0.2);
  border-radius: 999px;
}
</style>

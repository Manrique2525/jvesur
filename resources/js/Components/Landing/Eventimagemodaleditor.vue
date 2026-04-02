<template>
  <div class="min-h-screen bg-gray-950 text-white">
    <div class="max-w-7xl mx-auto px-4 py-8">

      <!-- Header -->
      <div class="flex flex-col sm:flex-row items-start sm:items-center justify-between mb-8 gap-4">
        <div>
          <h1 class="text-2xl font-bold text-white">Editor de Modal con Imagen</h1>
          <p class="text-gray-400 text-sm mt-1">Configura el modal de bienvenida con una imagen de evento</p>
        </div>
        <div class="flex items-center gap-3 flex-wrap">
          <button
            @click="resetToDefaults"
            class="px-4 py-2 rounded-lg border border-gray-700 text-gray-300 text-sm hover:bg-gray-800 transition-colors"
          >
            Restaurar
          </button>
          <button
            @click="showPreview = true"
            class="px-5 py-2 rounded-lg bg-[#C9A84C] text-[#1A2744] font-bold text-sm hover:bg-[#b8943e] transition-colors"
          >
            Vista Previa
          </button>
          <button
            @click="copyJson"
            class="px-4 py-2 rounded-lg border border-[#C9A84C]/50 text-[#C9A84C] text-sm hover:bg-[#C9A84C]/10 transition-colors"
          >
            {{ copied ? '¡Copiado!' : 'Copiar JSON' }}
          </button>
        </div>
      </div>

      <!-- Layout -->
      <div class="grid lg:grid-cols-2 gap-8">

        <!-- ═══════════════════════════════ -->
        <!-- PANEL DE EDICIÓN               -->
        <!-- ═══════════════════════════════ -->
        <div class="space-y-6">

          <!-- ── Imagen del Evento ── -->
          <div class="bg-gray-900 rounded-xl border border-gray-800 p-5">
            <div class="flex items-center gap-2 mb-4 pb-3 border-b border-gray-800">
              <span class="text-lg">🖼️</span>
              <h3 class="text-sm font-bold text-white tracking-wide">Imagen del Evento</h3>
            </div>

            <div class="mb-3">
              <label class="block text-xs text-gray-400 mb-1 font-medium">URL de la Imagen</label>
              <input v-model="form.image" type="text" class="editor-input" placeholder="/images/eventos/culto-aniversario.jpg" />
            </div>
            <div class="mb-3">
              <label class="block text-xs text-gray-400 mb-1 font-medium">Texto alternativo (accesibilidad)</label>
              <input v-model="form.alt" type="text" class="editor-input" placeholder="Culto de Aniversario - Domingo 05 de Abril" />
            </div>
            <div>
              <label class="block text-xs text-gray-400 mb-1 font-medium">Ancho máximo del modal</label>
              <select v-model="form.maxWidth" class="editor-input">
                <option value="400px">Pequeño (400px)</option>
                <option value="480px">Mediano (480px)</option>
                <option value="560px">Grande (560px)</option>
                <option value="640px">Extra grande (640px)</option>
              </select>
            </div>

            <!-- Preview de la imagen -->
            <div v-if="form.image" class="mt-4 rounded-lg overflow-hidden border border-gray-700">
              <img
                :src="form.image"
                :alt="form.alt"
                class="w-full h-auto block"
                @error="previewError = true"
                @load="previewError = false"
              />
              <div v-if="previewError" class="flex items-center justify-center py-12 bg-gray-800">
                <div class="text-center">
                  <svg class="w-8 h-8 text-red-400/50 mx-auto mb-2" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M12 9v2m0 4h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z"/>
                  </svg>
                  <p class="text-gray-500 text-xs">No se puede cargar la imagen</p>
                  <p class="text-gray-600 text-[10px] mt-1 break-all px-4">{{ form.image }}</p>
                </div>
              </div>
            </div>
            <div v-else class="mt-4 rounded-lg border border-dashed border-gray-700 py-12 flex items-center justify-center">
              <div class="text-center">
                <svg class="w-10 h-10 text-gray-700 mx-auto mb-2" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M4 16l4.586-4.586a2 2 0 012.828 0L16 16m-2-2l1.586-1.586a2 2 0 012.828 0L20 14m-6-6h.01M6 20h12a2 2 0 002-2V6a2 2 0 00-2-2H6a2 2 0 00-2 2v12a2 2 0 002 2z"/>
                </svg>
                <p class="text-gray-600 text-xs">Ingresa una URL de imagen arriba</p>
              </div>
            </div>
          </div>

          <!-- ── Enlace en Imagen ── -->
          <div class="bg-gray-900 rounded-xl border border-gray-800 p-5">
            <div class="flex items-center gap-2 mb-4 pb-3 border-b border-gray-800">
              <span class="text-lg">🔗</span>
              <h3 class="text-sm font-bold text-white tracking-wide">Enlace en Imagen</h3>
            </div>

            <label class="flex items-center gap-2 mb-3 cursor-pointer">
              <input type="checkbox" v-model="hasLink" class="accent-[#C9A84C]" />
              <span class="text-sm text-gray-300">La imagen es clickeable</span>
            </label>

            <template v-if="hasLink">
              <div class="mb-3">
                <label class="block text-xs text-gray-400 mb-1 font-medium">URL de destino</label>
                <input v-model="form.linkUrl" type="text" class="editor-input" placeholder="https://facebook.com/evento" />
              </div>
              <div>
                <label class="block text-xs text-gray-400 mb-1 font-medium">Abrir en</label>
                <select v-model="form.linkTarget" class="editor-input">
                  <option value="_blank">Nueva pestaña</option>
                  <option value="_self">Misma pestaña</option>
                </select>
              </div>
            </template>
          </div>

          <!-- ── Botón de Acción ── -->
          <div class="bg-gray-900 rounded-xl border border-gray-800 p-5">
            <div class="flex items-center gap-2 mb-4 pb-3 border-b border-gray-800">
              <span class="text-lg">🔘</span>
              <h3 class="text-sm font-bold text-white tracking-wide">Botón de Acción</h3>
            </div>

            <label class="flex items-center gap-2 mb-3 cursor-pointer">
              <input type="checkbox" v-model="hasAction" class="accent-[#C9A84C]" />
              <span class="text-sm text-gray-300">Mostrar botón debajo de la imagen</span>
            </label>

            <template v-if="hasAction">
              <div class="mb-3">
                <label class="block text-xs text-gray-400 mb-1 font-medium">Texto del botón</label>
                <input v-model="form.actionLabel" type="text" class="editor-input" placeholder="¡Quiero Asistir!" />
              </div>
              <div class="mb-3">
                <label class="block text-xs text-gray-400 mb-1 font-medium">URL o sección (#contacto)</label>
                <input v-model="form.actionUrl" type="text" class="editor-input" placeholder="#contacto" />
              </div>
              <div class="mb-3">
                <label class="block text-xs text-gray-400 mb-1 font-medium">Texto secundario debajo del botón (opcional)</label>
                <input v-model="form.actionSubtext" type="text" class="editor-input" placeholder="Domingo 05 de Abril — 10:00 AM" />
              </div>

              <!-- Colores del botón -->
              <div class="grid grid-cols-3 gap-3">
                <div>
                  <label class="block text-xs text-gray-400 mb-1 font-medium">Color botón</label>
                  <div class="flex items-center gap-2">
                    <input v-model="form.actionColor" type="color" class="w-9 h-9 rounded-lg border border-gray-700 cursor-pointer bg-transparent p-0.5" />
                    <input v-model="form.actionColor" type="text" class="editor-input font-mono text-xs" />
                  </div>
                </div>
                <div>
                  <label class="block text-xs text-gray-400 mb-1 font-medium">Color texto</label>
                  <div class="flex items-center gap-2">
                    <input v-model="form.actionTextColor" type="color" class="w-9 h-9 rounded-lg border border-gray-700 cursor-pointer bg-transparent p-0.5" />
                    <input v-model="form.actionTextColor" type="text" class="editor-input font-mono text-xs" />
                  </div>
                </div>
                <div>
                  <label class="block text-xs text-gray-400 mb-1 font-medium">Fondo footer</label>
                  <div class="flex items-center gap-2">
                    <input v-model="form.footerBg" type="color" class="w-9 h-9 rounded-lg border border-gray-700 cursor-pointer bg-transparent p-0.5" />
                    <input v-model="form.footerBg" type="text" class="editor-input font-mono text-xs" />
                  </div>
                </div>
              </div>
            </template>
          </div>

          <!-- ── Apariencia ── -->
          <div class="bg-gray-900 rounded-xl border border-gray-800 p-5">
            <div class="flex items-center gap-2 mb-4 pb-3 border-b border-gray-800">
              <span class="text-lg">🎨</span>
              <h3 class="text-sm font-bold text-white tracking-wide">Apariencia</h3>
            </div>

            <div>
              <label class="block text-xs text-gray-400 mb-1 font-medium">Estilo del botón cerrar (X)</label>
              <select v-model="form.closeButtonStyle" class="editor-input">
                <option value="dark">Oscuro (para imágenes claras)</option>
                <option value="light">Claro (para imágenes oscuras)</option>
              </select>
            </div>
          </div>

          <!-- ── Configuración ── -->
          <div class="bg-gray-900 rounded-xl border border-gray-800 p-5">
            <div class="flex items-center gap-2 mb-4 pb-3 border-b border-gray-800">
              <span class="text-lg">⚙️</span>
              <h3 class="text-sm font-bold text-white tracking-wide">Configuración</h3>
            </div>

            <div class="grid grid-cols-2 gap-3">
              <div>
                <label class="block text-xs text-gray-400 mb-1 font-medium">ID del Evento (único)</label>
                <input v-model="form.id" type="text" class="editor-input font-mono text-xs" placeholder="culto-aniversario-2026" />
              </div>
              <div>
                <label class="block text-xs text-gray-400 mb-1 font-medium">Delay para mostrar (ms)</label>
                <input v-model.number="form.delay" type="number" class="editor-input" min="0" step="100" />
              </div>
            </div>
            <label class="flex items-center gap-2 mt-3 cursor-pointer">
              <input type="checkbox" v-model="form.showDontShowAgain" class="accent-[#C9A84C]" />
              <span class="text-sm text-gray-300">Mostrar opción "No mostrar de nuevo"</span>
            </label>
          </div>

        </div>

        <!-- ═══════════════════════════════ -->
        <!-- PREVIEW EN VIVO (desktop)      -->
        <!-- ═══════════════════════════════ -->
        <div class="hidden lg:block">
          <div class="sticky top-8">
            <p class="text-xs text-gray-500 uppercase tracking-widest font-semibold mb-4">Vista previa en vivo</p>
            <div class="bg-gray-900 rounded-2xl p-6 border border-gray-800 flex items-center justify-center min-h-[400px]">
              <!-- Simulación del modal -->
              <div
                class="relative rounded-2xl overflow-hidden shadow-2xl bg-gray-800 w-full"
                :style="{ maxWidth: form.maxWidth }"
              >
                <!-- Close btn simulado -->
                <div
                  class="absolute top-3 right-3 z-30 w-9 h-9 rounded-full flex items-center justify-center"
                  :class="form.closeButtonStyle === 'light'
                    ? 'bg-white/80 text-gray-800 shadow-lg'
                    : 'bg-black/50 border border-white/20 text-white/60'"
                >
                  <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"/>
                  </svg>
                </div>

                <!-- Imagen -->
                <div v-if="form.image" class="relative">
                  <img
                    :src="form.image"
                    :alt="form.alt"
                    class="w-full h-auto block"
                    :class="{ 'cursor-pointer': hasLink }"
                    @error="previewError = true"
                    @load="previewError = false"
                  />
                </div>
                <div v-else class="flex items-center justify-center py-24">
                  <div class="text-center">
                    <svg class="w-12 h-12 text-gray-700 mx-auto mb-3" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                      <path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M4 16l4.586-4.586a2 2 0 012.828 0L16 16m-2-2l1.586-1.586a2 2 0 012.828 0L20 14m-6-6h.01M6 20h12a2 2 0 002-2V6a2 2 0 00-2-2H6a2 2 0 00-2 2v12a2 2 0 002 2z"/>
                    </svg>
                    <p class="text-gray-600 text-xs">Sin imagen</p>
                  </div>
                </div>

                <!-- Footer con botón -->
                <div
                  v-if="hasAction && form.actionLabel"
                  class="px-4 py-3"
                  :style="{ backgroundColor: form.footerBg }"
                >
                  <div
                    class="w-full py-2.5 rounded-lg text-center text-xs font-bold uppercase tracking-wider"
                    :style="{
                      backgroundColor: form.actionColor,
                      color: form.actionTextColor
                    }"
                  >
                    {{ form.actionLabel }}
                  </div>
                  <p
                    v-if="form.actionSubtext"
                    class="text-center text-[10px] mt-1.5 opacity-50"
                    :style="{ color: form.actionColor }"
                  >
                    {{ form.actionSubtext }}
                  </p>
                </div>
              </div>
            </div>
          </div>
        </div>

      </div>

      <!-- JSON Output -->
      <div class="mt-8 p-5 bg-gray-900 rounded-xl border border-gray-800">
        <div class="flex items-center justify-between mb-3">
          <p class="text-xs text-gray-500 uppercase tracking-widest font-semibold">Datos JSON (para backend)</p>
          <button
            @click="copyJson"
            class="text-xs text-[#C9A84C] hover:underline"
          >
            {{ copied ? '¡Copiado!' : 'Copiar' }}
          </button>
        </div>
        <pre class="text-xs text-gray-400 overflow-x-auto font-mono leading-relaxed">{{ jsonOutput }}</pre>
      </div>

    </div>

    <!-- Modal de Vista Previa real -->
    <EventImageModal
      v-if="showPreview"
      :modal="computedModalData"
      :auto-open="true"
      :delay="0"
      @close="showPreview = false"
    />
  </div>
</template>

<script setup>
import { ref, reactive, computed } from 'vue'
import EventImageModal from './Eventimagemodal.vue'

// ── Defaults ──
const getDefaults = () => ({
  id: 'culto-aniversario-2026',
  image: '/images/eventos/culto-aniversario.jpg',
  alt: 'Culto de Aniversario - Domingo 05 de Abril, 10:00 AM',
  maxWidth: '480px',
  linkUrl: '',
  linkTarget: '_blank',
  actionLabel: '¡Quiero Asistir!',
  actionUrl: '#contacto',
  actionColor: '#C9A84C',
  actionTextColor: '#1A2744',
  actionSubtext: 'Domingo 05 de Abril — 10:00 AM',
  footerBg: '#1A2744',
  closeButtonStyle: 'dark',
  showDontShowAgain: true,
  delay: 1500
})

const form = reactive(getDefaults())
const hasLink = ref(false)
const hasAction = ref(true)
const showPreview = ref(false)
const copied = ref(false)
const previewError = ref(false)

// ── Datos computados ──
const computedModalData = computed(() => ({
  ...form,
  linkUrl: hasLink.value ? form.linkUrl : null,
  actionLabel: hasAction.value ? form.actionLabel : null,
  actionUrl: hasAction.value ? form.actionUrl : null,
  actionSubtext: hasAction.value ? form.actionSubtext : null,
}))

const jsonOutput = computed(() => JSON.stringify(computedModalData.value, null, 2))

const copyJson = async () => {
  try {
    await navigator.clipboard.writeText(jsonOutput.value)
    copied.value = true
    setTimeout(() => { copied.value = false }, 2000)
  } catch {
    const el = document.createElement('textarea')
    el.value = jsonOutput.value
    document.body.appendChild(el)
    el.select()
    document.execCommand('copy')
    document.body.removeChild(el)
    copied.value = true
    setTimeout(() => { copied.value = false }, 2000)
  }
}

const resetToDefaults = () => {
  const defaults = getDefaults()
  Object.keys(defaults).forEach(key => {
    form[key] = defaults[key]
  })
  hasLink.value = false
  hasAction.value = true
}
</script>

<style scoped>
.editor-input {
  width: 100%;
  background-color: rgb(31 41 55);
  border: 1px solid rgb(55 65 81);
  border-radius: 0.5rem;
  padding: 0.5rem 0.75rem;
  font-size: 0.875rem;
  color: white;
  outline: none;
  transition: border-color 0.2s, box-shadow 0.2s;
}

.editor-input::placeholder {
  color: rgb(107 114 128);
}

.editor-input:focus {
  border-color: rgba(201, 168, 76, 0.5);
  box-shadow: 0 0 0 2px rgba(201, 168, 76, 0.15);
}
</style>

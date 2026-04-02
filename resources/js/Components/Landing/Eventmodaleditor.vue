<template>
  <div class="min-h-screen bg-gray-950 text-white">
    <div class="max-w-7xl mx-auto px-4 py-8">

      <!-- Header -->
      <div class="flex flex-col sm:flex-row items-start sm:items-center justify-between mb-8 gap-4">
        <div>
          <h1 class="text-2xl font-bold text-white">Editor de Evento</h1>
          <p class="text-gray-400 text-sm mt-1">Configura el modal de bienvenida para el próximo evento</p>
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

      <!-- Layout: Editor + Preview lado a lado en desktop -->
      <div class="grid lg:grid-cols-2 gap-8">

        <!-- ═══════════════════════════════════ -->
        <!-- PANEL DE EDICIÓN                    -->
        <!-- ═══════════════════════════════════ -->
        <div class="space-y-6">

          <!-- ── Sección: Información Principal ── -->
          <div class="bg-gray-900 rounded-xl border border-gray-800 p-5">
            <div class="flex items-center gap-2 mb-4 pb-3 border-b border-gray-800">
              <span class="text-lg">📋</span>
              <h3 class="text-sm font-bold text-white tracking-wide">Información Principal</h3>
            </div>

            <div class="mb-3">
              <label class="block text-xs text-gray-400 mb-1 font-medium">Etiqueta / Badge</label>
              <input v-model="form.badge" type="text" class="editor-input" placeholder="Evento Especial" />
            </div>
            <div class="mb-3">
              <label class="block text-xs text-gray-400 mb-1 font-medium">Título del Evento</label>
              <input v-model="form.title" type="text" class="editor-input" placeholder="Culto de Aniversario" />
            </div>
            <div class="mb-3">
              <label class="block text-xs text-gray-400 mb-1 font-medium">Subtítulo</label>
              <input v-model="form.subtitle" type="text" class="editor-input" placeholder="10mo Aniversario..." />
            </div>
            <div>
              <label class="block text-xs text-gray-400 mb-1 font-medium">Descripción</label>
              <textarea v-model="form.description" rows="3" class="editor-input resize-none" placeholder="Describe el evento..."></textarea>
            </div>
          </div>

          <!-- ── Sección: Fecha y Hora ── -->
          <div class="bg-gray-900 rounded-xl border border-gray-800 p-5">
            <div class="flex items-center gap-2 mb-4 pb-3 border-b border-gray-800">
              <span class="text-lg">📅</span>
              <h3 class="text-sm font-bold text-white tracking-wide">Fecha y Hora</h3>
            </div>

            <div class="grid grid-cols-2 gap-3">
              <div>
                <label class="block text-xs text-gray-400 mb-1 font-medium">Fecha</label>
                <input v-model="form.date" type="text" class="editor-input" placeholder="Domingo, 05 de Abril" />
              </div>
              <div>
                <label class="block text-xs text-gray-400 mb-1 font-medium">Info Extra Fecha</label>
                <input v-model="form.dateExtra" type="text" class="editor-input" placeholder="Próximo domingo" />
              </div>
              <div>
                <label class="block text-xs text-gray-400 mb-1 font-medium">Hora</label>
                <input v-model="form.time" type="text" class="editor-input" placeholder="10:00 AM" />
              </div>
              <div>
                <label class="block text-xs text-gray-400 mb-1 font-medium">Info Extra Hora</label>
                <input v-model="form.timeExtra" type="text" class="editor-input" placeholder="Puntualidad" />
              </div>
            </div>
          </div>

          <!-- ── Sección: Imagen de Cabecera ── -->
          <div class="bg-gray-900 rounded-xl border border-gray-800 p-5">
            <div class="flex items-center gap-2 mb-4 pb-3 border-b border-gray-800">
              <span class="text-lg">🖼️</span>
              <h3 class="text-sm font-bold text-white tracking-wide">Imagen de Cabecera</h3>
            </div>

            <div>
              <label class="block text-xs text-gray-400 mb-1 font-medium">URL de Imagen (dejar vacío para gradiente)</label>
              <input v-model="form.headerImage" type="text" class="editor-input" placeholder="/images/eventos/aniversario.jpg" />
            </div>
            <p class="text-gray-500 text-xs mt-2">Si no hay imagen, se mostrará un gradiente con los colores del tema.</p>
          </div>

          <!-- ── Sección: Invitado Especial ── -->
          <div class="bg-gray-900 rounded-xl border border-gray-800 p-5">
            <div class="flex items-center gap-2 mb-4 pb-3 border-b border-gray-800">
              <span class="text-lg">🎤</span>
              <h3 class="text-sm font-bold text-white tracking-wide">Invitado Especial</h3>
            </div>

            <label class="flex items-center gap-2 mb-3 cursor-pointer">
              <input type="checkbox" v-model="hasGuest" class="accent-[#C9A84C]" />
              <span class="text-sm text-gray-300">Tiene invitado especial</span>
            </label>

            <template v-if="hasGuest">
              <div class="mb-3">
                <label class="block text-xs text-gray-400 mb-1 font-medium">Nombre</label>
                <input v-model="form.specialGuest.name" type="text" class="editor-input" placeholder="Pastor Constantino Varas" />
              </div>
              <div class="mb-3">
                <label class="block text-xs text-gray-400 mb-1 font-medium">Cargo / Rol</label>
                <input v-model="form.specialGuest.role" type="text" class="editor-input" placeholder="Presidente de la CNBM" />
              </div>
              <div>
                <label class="block text-xs text-gray-400 mb-1 font-medium">URL de Foto (opcional)</label>
                <input v-model="form.specialGuest.image" type="text" class="editor-input" placeholder="/images/invitados/pastor.jpg" />
              </div>
            </template>
          </div>

          <!-- ── Sección: Ubicación ── -->
          <div class="bg-gray-900 rounded-xl border border-gray-800 p-5">
            <div class="flex items-center gap-2 mb-4 pb-3 border-b border-gray-800">
              <span class="text-lg">📍</span>
              <h3 class="text-sm font-bold text-white tracking-wide">Ubicación</h3>
            </div>

            <label class="flex items-center gap-2 mb-3 cursor-pointer">
              <input type="checkbox" v-model="hasLocation" class="accent-[#C9A84C]" />
              <span class="text-sm text-gray-300">Mostrar ubicación</span>
            </label>

            <template v-if="hasLocation">
              <div class="mb-3">
                <label class="block text-xs text-gray-400 mb-1 font-medium">Nombre del lugar</label>
                <input v-model="form.location.name" type="text" class="editor-input" placeholder="Iglesia Jesucristo es la Vida Eterna Sur" />
              </div>
              <div>
                <label class="block text-xs text-gray-400 mb-1 font-medium">Dirección</label>
                <textarea v-model="form.location.address" rows="2" class="editor-input resize-none" placeholder="Dirección completa..."></textarea>
              </div>
            </template>
          </div>

          <!-- ── Sección: Colores ── -->
          <div class="bg-gray-900 rounded-xl border border-gray-800 p-5">
            <div class="flex items-center gap-2 mb-4 pb-3 border-b border-gray-800">
              <span class="text-lg">🎨</span>
              <h3 class="text-sm font-bold text-white tracking-wide">Colores del Tema</h3>
            </div>

            <div class="grid grid-cols-1 sm:grid-cols-3 gap-4">
              <div>
                <label class="block text-xs text-gray-400 mb-1 font-medium">Principal</label>
                <div class="flex items-center gap-2">
                  <input v-model="form.colors.primary" type="color" class="w-10 h-10 rounded-lg border border-gray-700 cursor-pointer bg-transparent p-0.5" />
                  <input v-model="form.colors.primary" type="text" class="editor-input font-mono text-xs" />
                </div>
              </div>
              <div>
                <label class="block text-xs text-gray-400 mb-1 font-medium">Oscuro</label>
                <div class="flex items-center gap-2">
                  <input v-model="form.colors.primaryDark" type="color" class="w-10 h-10 rounded-lg border border-gray-700 cursor-pointer bg-transparent p-0.5" />
                  <input v-model="form.colors.primaryDark" type="text" class="editor-input font-mono text-xs" />
                </div>
              </div>
              <div>
                <label class="block text-xs text-gray-400 mb-1 font-medium">Acento</label>
                <div class="flex items-center gap-2">
                  <input v-model="form.colors.accent" type="color" class="w-10 h-10 rounded-lg border border-gray-700 cursor-pointer bg-transparent p-0.5" />
                  <input v-model="form.colors.accent" type="text" class="editor-input font-mono text-xs" />
                </div>
              </div>
            </div>
          </div>

          <!-- ── Sección: Botones de Acción ── -->
          <div class="bg-gray-900 rounded-xl border border-gray-800 p-5">
            <div class="flex items-center gap-2 mb-4 pb-3 border-b border-gray-800">
              <span class="text-lg">🔘</span>
              <h3 class="text-sm font-bold text-white tracking-wide">Botones de Acción</h3>
            </div>

            <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
              <div>
                <p class="text-xs text-[#C9A84C] font-semibold uppercase tracking-wider mb-2">Botón Principal</p>
                <div class="mb-3">
                  <label class="block text-xs text-gray-400 mb-1 font-medium">Texto</label>
                  <input v-model="form.primaryAction.label" type="text" class="editor-input" placeholder="¡Quiero Asistir!" />
                </div>
                <div>
                  <label class="block text-xs text-gray-400 mb-1 font-medium">Enlace / Sección</label>
                  <input v-model="form.primaryAction.url" type="text" class="editor-input" placeholder="#contacto" />
                </div>
              </div>
              <div>
                <p class="text-xs text-gray-400 font-semibold uppercase tracking-wider mb-2">Botón Secundario</p>
                <div class="mb-3">
                  <label class="block text-xs text-gray-400 mb-1 font-medium">Texto</label>
                  <input v-model="form.secondaryAction.label" type="text" class="editor-input" placeholder="Más Información" />
                </div>
                <div>
                  <label class="block text-xs text-gray-400 mb-1 font-medium">Enlace / Sección</label>
                  <input v-model="form.secondaryAction.url" type="text" class="editor-input" placeholder="#servicios" />
                </div>
              </div>
            </div>
          </div>

          <!-- ── Sección: Configuración ── -->
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

        <!-- ═══════════════════════════════════ -->
        <!-- PREVIEW EN VIVO (desktop)           -->
        <!-- ═══════════════════════════════════ -->
        <div class="hidden lg:block">
          <div class="sticky top-8">
            <p class="text-xs text-gray-500 uppercase tracking-widest font-semibold mb-4">Vista previa en vivo</p>
            <div class="bg-gray-900 rounded-2xl p-6 border border-gray-800">
              <div class="relative w-full rounded-2xl overflow-hidden shadow-2xl">
                <!-- Close btn simulado -->
                <div class="absolute top-3 right-3 z-30 w-9 h-9 rounded-full bg-black/50 border border-white/20 flex items-center justify-center text-white/60">
                  <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"/></svg>
                </div>

                <!-- Header -->
                <div class="relative h-44 overflow-hidden">
                  <div
                    v-if="form.headerImage"
                    class="absolute inset-0 bg-cover bg-center"
                    :style="{ backgroundImage: `url(${form.headerImage})` }"
                  ></div>
                  <div v-else class="absolute inset-0" :style="{ background: `linear-gradient(135deg, ${form.colors.primary}, ${form.colors.primaryDark})` }">
                    <div class="absolute inset-0 opacity-10" style="background-image: radial-gradient(circle, rgba(255,255,255,0.3) 1px, transparent 1px); background-size: 24px 24px;"></div>
                  </div>
                  <div class="absolute inset-0" :style="{ background: `linear-gradient(to top, ${form.colors.primary}, transparent 60%)` }"></div>

                  <div class="absolute top-3 left-3 z-10">
                    <span class="inline-flex items-center gap-1 px-2 py-0.5 rounded-full text-[9px] tracking-widest uppercase font-bold border"
                      :style="{ backgroundColor: `${form.colors.accent}20`, borderColor: `${form.colors.accent}50`, color: form.colors.accent }">
                      <span class="w-1 h-1 rounded-full" :style="{ backgroundColor: form.colors.accent }"></span>
                      {{ form.badge || 'Badge' }}
                    </span>
                  </div>

                  <div class="absolute bottom-3 left-4 right-4 z-10">
                    <h2 class="text-xl font-extrabold" :style="{ color: form.colors.accent }">{{ form.title || 'Título' }}</h2>
                    <p v-if="form.subtitle" class="text-xs text-white/70 mt-0.5">{{ form.subtitle }}</p>
                  </div>
                </div>

                <!-- Body preview -->
                <div class="px-4 pt-4 pb-5" :style="{ backgroundColor: form.colors.primary }">
                  <p class="text-white/60 text-xs leading-relaxed mb-4 line-clamp-2">{{ form.description || 'Descripción...' }}</p>

                  <div
                    v-if="hasGuest && form.specialGuest.name"
                    class="flex items-center gap-2 mb-4 p-2.5 rounded-lg border"
                    :style="{ borderColor: `${form.colors.accent}25`, backgroundColor: `${form.colors.accent}08` }"
                  >
                    <div class="w-8 h-8 rounded-full border flex items-center justify-center text-xs font-bold flex-shrink-0"
                      :style="{ borderColor: form.colors.accent, color: form.colors.accent, backgroundColor: `${form.colors.accent}20` }">
                      {{ form.specialGuest.name.charAt(0) }}
                    </div>
                    <div class="min-w-0">
                      <p class="text-white font-bold text-xs truncate">{{ form.specialGuest.name }}</p>
                      <p class="text-white/40 text-[10px] truncate">{{ form.specialGuest.role }}</p>
                    </div>
                  </div>

                  <div class="space-y-1.5 mb-4">
                    <div class="flex items-center gap-2 text-xs">
                      <span>📅</span>
                      <span class="text-white/80">{{ form.date || 'Fecha' }}</span>
                    </div>
                    <div class="flex items-center gap-2 text-xs">
                      <span>🕐</span>
                      <span class="text-white/80">{{ form.time || 'Hora' }}</span>
                    </div>
                    <div v-if="hasLocation" class="flex items-center gap-2 text-xs">
                      <span>📍</span>
                      <span class="text-white/80 truncate">{{ form.location.name || 'Ubicación' }}</span>
                    </div>
                  </div>

                  <div class="flex gap-2">
                    <div class="flex-1 py-2 rounded-lg text-center text-xs font-bold uppercase tracking-wider"
                      :style="{ backgroundColor: form.colors.accent, color: form.colors.primary }">
                      {{ form.primaryAction.label || 'Acción' }}
                    </div>
                    <div class="flex-1 py-2 rounded-lg text-center text-xs font-bold uppercase tracking-wider border"
                      :style="{ borderColor: `${form.colors.accent}40`, color: form.colors.accent }">
                      {{ form.secondaryAction.label || 'Secundario' }}
                    </div>
                  </div>
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
    <EventModal
      v-if="showPreview"
      :event="computedEventData"
      :auto-open="true"
      :delay="0"
      @close="showPreview = false"
    />
  </div>
</template>

<script setup>
import { ref, reactive, computed } from 'vue'
import EventModal from './EventModal.vue'

// ── Estado del formulario ──
const getDefaults = () => ({
  id: 'culto-aniversario-2026',
  badge: 'Evento Especial',
  title: 'Culto de Aniversario',
  subtitle: '10mo Aniversario de nuestra iglesia',
  description: 'Ven y celebra con nosotros nuestro 10mo. Aniversario donde tendremos un culto especial, llenos de alegría y gozo.',
  date: 'Domingo, 05 de Abril 2026',
  dateExtra: 'Próximo domingo',
  time: '10:00 AM',
  timeExtra: 'Puntualidad',
  headerImage: '',
  specialGuest: {
    name: 'Pastor Constantino Varas',
    role: 'Presidente de la CNBM',
    image: ''
  },
  location: {
    name: 'Iglesia Jesucristo es la Vida Eterna Sur',
    address: 'Carret. a Teapa a 100 mts. del Periférico Carlos Pellicer Cámara, frente a CITY CLUB, Col. Plutarco Elías Calles'
  },
  colors: {
    primary: '#1A2744',
    primaryDark: '#0f1a2e',
    accent: '#C9A84C'
  },
  primaryAction: {
    label: '¡Quiero Asistir!',
    url: '#contacto'
  },
  secondaryAction: {
    label: 'Más Información',
    url: '#servicios'
  },
  showDontShowAgain: true,
  delay: 1500
})

const form = reactive(getDefaults())
const hasGuest = ref(true)
const hasLocation = ref(true)
const showPreview = ref(false)
const copied = ref(false)

// ── Datos computados para pasar al modal ──
const computedEventData = computed(() => ({
  ...form,
  headerImage: form.headerImage || null,
  specialGuest: hasGuest.value ? {
    ...form.specialGuest,
    image: form.specialGuest.image || null
  } : null,
  location: hasLocation.value ? form.location : null,
}))

// ── JSON output ──
const jsonOutput = computed(() => JSON.stringify(computedEventData.value, null, 2))

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
    if (typeof defaults[key] === 'object' && defaults[key] !== null) {
      Object.assign(form[key], defaults[key])
    } else {
      form[key] = defaults[key]
    }
  })
  hasGuest.value = true
  hasLocation.value = true
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

.line-clamp-2 {
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
}
</style>

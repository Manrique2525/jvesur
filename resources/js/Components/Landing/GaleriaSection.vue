<template>
  <section id="galeria" class="py-24 bg-[#FAF7F2] relative overflow-hidden">
    <div class="absolute top-1/3 right-0 w-72 h-72 bg-[#C9A84C]/5 rounded-full blur-3xl pointer-events-none"></div>
    <div class="absolute bottom-0 left-0 w-64 h-64 bg-[#1A2744]/5 rounded-full blur-3xl pointer-events-none"></div>

    <div class="relative z-10 max-w-7xl mx-auto px-6">
      <div class="text-center mb-16">
        <span class="inline-block text-[#C9A84C] text-xs tracking-[0.4em] uppercase font-semibold mb-3">Memorias</span>
        <h2 class="text-4xl md:text-5xl font-bold text-[#1A2744] mb-4">Galería de Fotos y Videos</h2>
        <div class="w-16 h-0.5 bg-[#C9A84C] mx-auto"></div>
        <p class="text-gray-500 mt-6 max-w-xl mx-auto">
          Revive los momentos especiales de nuestra comunidad. Un espacio dedicado a nuestra historia y fe.
        </p>
      </div>

      <div class="mb-24">
        <div class="flex items-center justify-between mb-8">
          <h3 class="text-2xl font-bold text-[#1A2744] flex items-center gap-2">
            <svg class="w-6 h-6 text-[#C9A84C]" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 16l4.586-4.586a2 2 0 012.828 0L16 16m-2-2l1.586-1.586a2 2 0 012.828 0L20 14m-6-6h.01M6 20h12a2 2 0 002-2V6a2 2 0 00-2-2H6a2 2 0 00-2 2v12a2 2 0 002 2z" />
            </svg>
            Fotografías
          </h3>
          <span class="text-sm text-gray-400">{{ photos.length }} imágenes</span>
        </div>

        <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6">
          <TransitionGroup name="fade-grid">
            <div
              v-for="photo in photos"
              :key="photo.id"
              class="group relative rounded-xl overflow-hidden shadow-sm hover:shadow-xl transition-all duration-500 cursor-pointer bg-white"
              @click="openLightbox(photo.url, photo.title)"
            >
              <div class="relative aspect-[4/3] overflow-hidden">
                <img :src="photo.url" :alt="photo.title" class="w-full h-full object-cover transition-transform duration-700 group-hover:scale-110" loading="lazy" />
                <div class="absolute inset-0 bg-black/0 group-hover:bg-black/30 transition-colors duration-300 flex items-center justify-center">
                  <svg class="w-12 h-12 text-white opacity-0 group-hover:opacity-100 transition-all duration-300 transform scale-75 group-hover:scale-100" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0zM10 7v3m0 0v3m0-3h3m-3 0H7" />
                  </svg>
                </div>
              </div>
              <div class="p-4">
                <h4 class="font-semibold text-[#1A2744] line-clamp-1">{{ photo.title }}</h4>
                <p class="text-xs text-gray-400 mt-1">{{ photo.date }}</p>
              </div>
            </div>
          </TransitionGroup>
        </div>

        <div v-if="hasMorePhotos" class="text-center mt-12">
          <button
            @click="simulateLoadPhotos"
            :disabled="loadingPhotos"
            class="inline-flex items-center gap-3 px-8 py-3 border-2 border-[#C9A84C] text-[#C9A84C] rounded-full text-sm font-bold hover:bg-[#C9A84C] hover:text-[#1A2744] transition-all duration-300 disabled:opacity-50"
          >
            <span v-if="loadingPhotos" class="animate-spin text-lg">◌</span>
            {{ loadingPhotos ? 'Cargando Galería...' : 'Cargar más fotos' }}
          </button>
        </div>
      </div>

      <div>
        <div class="flex items-center justify-between mb-8">
          <h3 class="text-2xl font-bold text-[#1A2744] flex items-center gap-2">
            <svg class="w-6 h-6 text-[#C9A84C]" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 10l4.553-2.276A1 1 0 0121 8.618v6.764a1 1 0 01-1.447.894L15 14M5 18h8a2 2 0 002-2V8a2 2 0 00-2-2H5a2 2 0 00-2 2v12a2 2 0 002 2z" />
            </svg>
            Videos de la Comunidad
          </h3>
        </div>

        <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6">
          <TransitionGroup name="fade-grid">
            <div
              v-for="video in videos"
              :key="video.id"
              class="group rounded-xl overflow-hidden shadow-sm hover:shadow-xl transition-all duration-500 cursor-pointer bg-white"
              @click="openVideoModal(video)"
            >
              <div class="relative aspect-video overflow-hidden bg-[#1A2744]">

                <img
                  :src="video.customThumbnail || `https://img.youtube.com/vi/${video.youtubeId}/mqdefault.jpg`"
                  :alt="video.title"
                  class="w-full h-full object-cover transition-transform duration-700 group-hover:scale-105"
                />

                <div class="absolute inset-0 bg-black/20 group-hover:bg-black/50 transition-colors flex items-center justify-center">
                  <div class="w-14 h-14 rounded-full bg-[#C9A84C] flex items-center justify-center shadow-lg transform group-hover:scale-110 transition-transform duration-300">
                    <svg class="w-6 h-6 text-[#1A2744] ml-0.5" fill="currentColor" viewBox="0 0 24 24"><path d="M8 5v14l11-7z" /></svg>
                  </div>
                </div>
              </div>
              <div class="p-4">
                <h4 class="font-semibold text-[#1A2744] line-clamp-1">{{ video.title }}</h4>
                <p class="text-xs text-gray-400 mt-1">Video de Playlist</p>
              </div>
            </div>
          </TransitionGroup>
        </div>

        <div v-if="hasMoreVideos" class="text-center mt-12">
          <button
            @click="simulateLoadVideos"
            :disabled="loadingVideos"
            class="inline-flex items-center gap-2 text-[#C9A84C] font-bold hover:underline disabled:opacity-50"
          >
            {{ loadingVideos ? 'Consultando Playlist...' : 'Ver más videos' }}
            <svg v-if="!loadingVideos" class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 9l-7 7-7-7" /></svg>
          </button>
        </div>
      </div>
    </div>

    <Teleport to="body">
      <Transition name="modal-fade">
        <div v-if="lightbox.open" class="fixed inset-0 z-[9999] flex items-center justify-center p-4 bg-black/95 backdrop-blur-sm" @click.self="closeLightbox">
          <div class="relative max-w-5xl w-full">
            <button @click="closeLightbox" class="absolute -top-12 right-0 text-white hover:text-[#C9A84C] transition-colors">
              <svg class="w-8 h-8" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" /></svg>
            </button>
            <img :src="lightbox.image" class="w-full h-auto max-h-[80vh] object-contain rounded-lg" />
            <p class="text-center text-white mt-4 text-lg font-medium">{{ lightbox.title }}</p>
          </div>
        </div>
      </Transition>
    </Teleport>

    <Teleport to="body">
      <Transition name="modal-fade">
        <div v-if="videoModal.open" class="fixed inset-0 z-[9999] flex items-center justify-center p-4 bg-black/95 backdrop-blur-sm" @click.self="closeVideoModal">
          <div class="relative w-full max-w-4xl overflow-hidden rounded-2xl shadow-2xl bg-black">
            <button @click="closeVideoModal" class="absolute top-4 right-4 z-20 text-white/50 hover:text-white transition-colors">
              <svg class="w-8 h-8" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" /></svg>
            </button>
            <div class="relative pb-[56.25%] h-0">
              <iframe class="absolute top-0 left-0 w-full h-full" :src="videoModal.url" frameborder="0" allow="autoplay; encrypted-media; picture-in-picture" allowfullscreen></iframe>
            </div>
          </div>
        </div>
      </Transition>
    </Teleport>
  </section>
</template>

<script setup>
import { ref, reactive, onMounted, onUnmounted } from 'vue'

// --- CONFIGURACIÓN DE LA PLAYLIST ---
const PLAYLIST_ID = 'PLinCweJZNZ1F-VzP5-5xdtOY9l9mmmULs'

// --- ESTADOS DE FOTOS ---
const photos = ref([
  { id: 1, url: 'https://images.unsplash.com/photo-1511632765486-a01980e01a18?w=800', title: 'Culto de Jóvenes', date: 'Marzo 2026' },
  { id: 2, url: 'https://images.unsplash.com/photo-1504052434569-70ad5836ab65?w=800', title: 'Bautismos', date: 'Febrero 2026' },
  { id: 3, url: '/images/image3.jpg', title: 'Alabanza y Adoración', date: 'Enero 2026' }
])
const loadingPhotos = ref(false)
const hasMorePhotos = ref(true)

// --- ESTADOS DE VIDEOS (IDs extraídos de tu playlist) ---
const videos = ref([
  {
    id: 101,
    title: 'Momento de Reflexión',
    youtubeId: '95UvS0Plk_A',
    // // Pon aquí la URL de la imagen que quieras para este video (ej: de Unsplash o tu carpeta assets)
   // customThumbnail: 'https://images.unsplash.com/photo-1501281668745-f7f57925c3b6?w=800&q=80'
  },
  {
    id: 102,
    title: 'Entrevista Especial',
    youtubeId: 'rA1VRugP074',
    // customThumbnail: 'https://images.unsplash.com/photo-1517694712202-14dd9538aa97?w=800&q=80'
  },
  {
    id: 103,
    title: 'Resumen del Evento',
    youtubeId: 'WXxsyku1AUk',
    // // Si lo dejas en null o vacío, usará la de YouTube por defecto
    customThumbnail: null
  }
])
const loadingVideos = ref(false)
const hasMoreVideos = ref(true)

// --- MODALES ---
const lightbox = reactive({ open: false, image: '', title: '' })
const videoModal = reactive({ open: false, url: '' })

// --- LÓGICA DE CARGA SIMULADA ---
const simulateLoadPhotos = async () => {
  loadingPhotos.value = true
  await new Promise(r => setTimeout(r, 1500))
  photos.value.push(
    { id: Date.now(), url: '/images/image4.jpg', title: 'Retiro Espiritual', date: 'Abril 2026' },
    { id: Date.now() + 1, url: '/images/image5.jpg', title: 'Santa cena', date: 'Abril 2026' }
  )
  loadingPhotos.value = false
  if (photos.value.length >= 7) hasMorePhotos.value = false
}

const simulateLoadVideos = async () => {
  loadingVideos.value = true
  await new Promise(r => setTimeout(r, 1500))
  // Agregamos más videos
  videos.value.push(
    {
      id: Date.now(),
      title: 'Testimonio en Vivo',
      youtubeId: 'rA1VRugP074',
      // customThumbnail: 'https://images.unsplash.com/photo-1492684223066-81342ee5ff30?w=800&q=80'
    },
    {
      id: Date.now() + 1,
      title: 'Coro de Niños',
      youtubeId: 'rA1VRugP074',
      // customThumbnail: 'https://images.unsplash.com/photo-1471960666630-a15d3be72644?w=800&q=80'
    }
  )
  loadingVideos.value = false
  if (videos.value.length >= 6) hasMoreVideos.value = false
}

// --- ACCIONES DE MODAL ---
const openLightbox = (url, title) => {
  lightbox.image = url; lightbox.title = title; lightbox.open = true
  document.body.style.overflow = 'hidden'
}
const closeLightbox = () => {
  lightbox.open = false; document.body.style.overflow = ''
}

const openVideoModal = (v) => {
  // Cargamos el video específico pero vinculado a tu playlist
  videoModal.url = `https://www.youtube.com/embed/${v.youtubeId}?autoplay=1&list=${PLAYLIST_ID}&rel=0`
  videoModal.open = true
  document.body.style.overflow = 'hidden'
}
const closeVideoModal = () => {
  videoModal.open = false; document.body.style.overflow = ''
}

// --- EVENTOS ---
const handleEsc = (e) => {
  if (e.key === 'Escape') { closeLightbox(); closeVideoModal() }
}
onMounted(() => window.addEventListener('keydown', handleEsc))
onUnmounted(() => window.removeEventListener('keydown', handleEsc))
</script>

<style scoped>
.line-clamp-1 {
  display: -webkit-box;
  -webkit-line-clamp: 1;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

/* Transición para los items del grid */
.fade-grid-enter-active {
  transition: all 0.6s ease-out;
}
.fade-grid-enter-from {
  opacity: 0;
  transform: translateY(20px) scale(0.95);
}

/* Transición para los modales */
.modal-fade-enter-active, .modal-fade-leave-active {
  transition: opacity 0.4s ease;
}
.modal-fade-enter-from, .modal-fade-leave-to {
  opacity: 0;
}
</style>

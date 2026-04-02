<template>
  <footer class="bg-gradient-to-br from-[#007FE1] to-[#00E0A6] text-white py-14">
    <div class="max-w-7xl mx-auto px-6">

      <div class="grid md:grid-cols-3 gap-10 pb-10 border-b border-white/20">

        <!-- Brand -->
        <div>
          <div class="flex items-center gap-3 mb-4">
            <div class="w-9 h-9 rounded-full border-2 border-white flex items-center justify-center">
              <svg class="w-4 h-4 text-white" fill="currentColor" viewBox="0 0 24 24">
                <path d="M12 2L9 9H2l5.5 4-2 7L12 16l6.5 4-2-7L22 9h-7z"/>
              </svg>
            </div>
            <div>
              <p class="text-white font-bold text-sm tracking-widest uppercase">Jesucristo</p>
              <p class="text-white/80 text-xs">Es La Vida Eterna Sur</p>
            </div>
          </div>
          <p class="text-white/80 text-sm leading-relaxed mb-4">
            Una comunidad de fe comprometida con el amor de Dios y el servicio a nuestra ciudad.
          </p>
          <blockquote class="border-l-2 border-white/50 pl-4 text-white/80 text-xs italic">
            "Porque de tal manera amó Dios al mundo..." — Juan 3:16
          </blockquote>
        </div>

        <!-- Quick links -->
        <div>
          <h4 class="text-white font-bold text-xs tracking-[0.3em] uppercase mb-5">Navegación</h4>
          <ul class="space-y-2">
            <li v-for="link in links" :key="link.label">
              <a
                :href="link.href"
                @click.prevent="scrollTo(link.href)"
                class="text-white/80 hover:text-white text-sm transition-colors"
              >{{ link.label }}</a>
            </li>
          </ul>
        </div>

        <!-- Contact -->
        <div>
          <h4 class="text-white font-bold text-xs tracking-[0.3em] uppercase mb-5">Contacto</h4>
          <div class="space-y-3">
            <p class="text-white/80 text-sm flex items-center gap-2">
              <span>📍</span>
              Villahermosa, Tabasco, México
            </p>
            <p class="text-white/80 text-sm flex items-center gap-2">
              <span>📞</span>
              <a href="tel:+529930000000" class="hover:text-white transition-colors">+52 993 000 0000</a>
            </p>
            <p class="text-white/80 text-sm flex items-center gap-2">
              <span>✉️</span>
              <a href="mailto:contacto@iglesia.com" class="hover:text-white transition-colors">contacto@iglesia.com</a>
            </p>
          </div>

          <!-- Social -->
          <div class="flex items-center gap-3 mt-6">
            <a
              v-for="s in socialIcons"
              :key="s.name"
              :href="s.url"
              target="_blank"
              :title="s.name"
              class="w-8 h-8 rounded-full border border-white/30 flex items-center justify-center text-white/70 hover:bg-white/20 hover:text-white transition-all duration-300"
            >
              <svg class="w-4 h-4" fill="currentColor" viewBox="0 0 24 24">
                <path :d="s.path"/>
              </svg>
            </a>
          </div>
        </div>
      </div>

      <!-- Bottom -->
      <div class="pt-6 flex flex-col sm:flex-row items-center justify-between gap-3">
        <p class="text-white/70 text-xs">
          © {{ year }} Jesucristo es la Vida Eterna Sur. Todos los derechos reservados.
        </p>
        <p class="text-white/60 text-xs">
          Hecho con fe y <span class="text-white">♥</span>
        </p>
      </div>
    </div>
  </footer>
</template>

<script setup>
import { computed } from 'vue'

const year = computed(() => new Date().getFullYear())

const links = [
  { label: 'Inicio', href: '#inicio' },
  { label: 'Nosotros', href: '#nosotros' },
  { label: 'Servicios', href: '#servicios' },
  { label: 'Redes Sociales', href: '#redes' },
  { label: 'Contáctanos', href: '#contacto' },
  { label: 'Ubicación', href: '#ubicacion' },
]

const socialIcons = [
  {
    name: 'Facebook',
    url: 'https://facebook.com',
    path: 'M18 2h-3a5 5 0 00-5 5v3H7v4h3v8h4v-8h3l1-4h-4V7a1 1 0 011-1h3z',
  },
  {
    name: 'Instagram',
    url: 'https://instagram.com',
    path: 'M12 2.163c3.204 0 3.584.012 4.85.07...',
  },
  {
    name: 'YouTube',
    url: 'https://youtube.com',
    path: 'M23.495 6.205a3.007 3.007 0 00-2.088...',
  },
  {
    name: 'WhatsApp',
    url: 'https://wa.me/529930000000',
    path: 'M17.472 14.382c-.297-.149...',
  },
]

const scrollTo = (href) => {
  const el = document.querySelector(href)
  if (el) el.scrollIntoView({ behavior: 'smooth' })
}
</script>
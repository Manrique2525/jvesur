<template>
  <footer class="bg-[#0F1A36] text-white py-14">
    <div class="max-w-7xl mx-auto px-6">

      <div class="grid md:grid-cols-3 gap-10 pb-10 border-b border-white/10">

        <!-- Brand -->
        <div>
          <div class="flex items-center gap-3 mb-4">
            <div class="w-9 h-9 rounded-full border-2 border-[#C9A84C] flex items-center justify-center">
              <svg class="w-4 h-4 text-[#C9A84C]" fill="currentColor" viewBox="0 0 24 24">
                <path d="M12 2L9 9H2l5.5 4-2 7L12 16l6.5 4-2-7L22 9h-7z"/>
              </svg>
            </div>
            <div>
              <p class="text-[#C9A84C] font-bold text-sm tracking-widest uppercase">Jesucristo</p>
              <p class="text-white/50 text-xs">Es La Vida Eterna Sur</p>
            </div>
          </div>
          <p class="text-white/40 text-sm leading-relaxed mb-4">
            Una comunidad de fe comprometida con el amor de Dios y el servicio a nuestra ciudad.
          </p>
          <blockquote class="border-l-2 border-[#C9A84C]/40 pl-4 text-white/40 text-xs italic">
            "Porque de tal manera amó Dios al mundo..." — Juan 3:16
          </blockquote>
        </div>

        <!-- Quick links -->
        <div>
          <h4 class="text-[#C9A84C] font-bold text-xs tracking-[0.3em] uppercase mb-5">Navegación</h4>
          <ul class="space-y-2">
            <li v-for="link in links" :key="link.label">
              <a
                :href="link.href"
                @click.prevent="scrollTo(link.href)"
                class="text-white/50 hover:text-[#C9A84C] text-sm transition-colors"
              >{{ link.label }}</a>
            </li>
          </ul>
        </div>

        <!-- Contact quick info -->
        <div>
          <h4 class="text-[#C9A84C] font-bold text-xs tracking-[0.3em] uppercase mb-5">Contacto</h4>
          <div class="space-y-3">
            <p class="text-white/50 text-sm flex items-center gap-2">
              <span class="text-[#C9A84C]">📍</span>
              Villahermosa, Tabasco, México
            </p>
            <p class="text-white/50 text-sm flex items-center gap-2">
              <span class="text-[#C9A84C]">📞</span>
              <a href="tel:+529930000000" class="hover:text-white transition-colors">+52 993 000 0000</a>
            </p>
            <p class="text-white/50 text-sm flex items-center gap-2">
              <span class="text-[#C9A84C]">✉️</span>
              <a href="mailto:contacto@iglesia.com" class="hover:text-white transition-colors">contacto@iglesia.com</a>
            </p>
          </div>

          <!-- Social icons small -->
          <div class="flex items-center gap-3 mt-6">
            <a
              v-for="s in socialIcons"
              :key="s.name"
              :href="s.url"
              target="_blank"
              :title="s.name"
              class="w-8 h-8 rounded-full border border-white/10 flex items-center justify-center text-white/40 hover:border-[#C9A84C]/50 hover:text-[#C9A84C] transition-all duration-300"
            >
              <svg class="w-4 h-4" fill="currentColor" viewBox="0 0 24 24">
                <path :d="s.path"/>
              </svg>
            </a>
          </div>
        </div>
      </div>

      <!-- Bottom bar -->
      <div class="pt-6 flex flex-col sm:flex-row items-center justify-between gap-3">
        <p class="text-white/30 text-xs">© {{ year }} Jesucristo es la Vida Eterna Sur. Todos los derechos reservados.</p>
        <p class="text-white/20 text-xs">Hecho con fe y <span class="text-[#C9A84C]">♥</span></p>
      </div>
    </div>
  </footer>
</template>

<script setup>
import { computed } from 'vue'

const year = computed(() => new Date().getFullYear())

const links = [
  { label: 'Inicio',        href: '#inicio' },
  { label: 'Nosotros',      href: '#nosotros' },
  { label: 'Servicios',     href: '#servicios' },
  { label: 'Redes Sociales',href: '#redes' },
  { label: 'Contáctanos',   href: '#contacto' },
  { label: 'Ubicación',     href: '#ubicacion' },
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
    path: 'M12 2.163c3.204 0 3.584.012 4.85.07 3.252.148 4.771 1.691 4.919 4.919.058 1.265.069 1.645.069 4.849 0 3.205-.012 3.584-.069 4.849-.149 3.225-1.664 4.771-4.919 4.919-1.266.058-1.644.07-4.85.07-3.204 0-3.584-.012-4.849-.07-3.26-.149-4.771-1.699-4.919-4.92-.058-1.265-.07-1.644-.07-4.849 0-3.204.013-3.583.07-4.849.149-3.227 1.664-4.771 4.919-4.919 1.266-.057 1.645-.069 4.849-.069zm0-2.163c-3.259 0-3.667.014-4.947.072-4.358.2-6.78 2.618-6.98 6.98-.059 1.281-.073 1.689-.073 4.948 0 3.259.014 3.668.072 4.948.2 4.358 2.618 6.78 6.98 6.98 1.281.058 1.689.072 4.948.072 3.259 0 3.668-.014 4.948-.072 4.354-.2 6.782-2.618 6.979-6.98.059-1.28.073-1.689.073-4.948 0-3.259-.014-3.667-.072-4.947-.196-4.354-2.617-6.78-6.979-6.98-1.281-.059-1.69-.073-4.949-.073zm0 5.838c-3.403 0-6.162 2.759-6.162 6.162s2.759 6.163 6.162 6.163 6.162-2.759 6.162-6.163c0-3.403-2.759-6.162-6.162-6.162zm0 10.162c-2.209 0-4-1.79-4-4 0-2.209 1.791-4 4-4s4 1.791 4 4c0 2.21-1.791 4-4 4zm6.406-11.845c-.796 0-1.441.645-1.441 1.44s.645 1.44 1.441 1.44c.795 0 1.439-.645 1.439-1.44s-.644-1.44-1.439-1.44z',
  },
  {
    name: 'YouTube',
    url: 'https://youtube.com',
    path: 'M23.495 6.205a3.007 3.007 0 00-2.088-2.088c-1.87-.501-9.396-.501-9.396-.501s-7.507-.01-9.396.501A3.007 3.007 0 00.527 6.205a31.247 31.247 0 00-.522 5.805 31.247 31.247 0 00.522 5.783 3.007 3.007 0 002.088 2.088c1.868.502 9.396.502 9.396.502s7.506 0 9.396-.502a3.007 3.007 0 002.088-2.088 31.247 31.247 0 00.5-5.783 31.247 31.247 0 00-.5-5.805zM9.609 15.601V8.408l6.264 3.602z',
  },
  {
    name: 'WhatsApp',
    url: 'https://wa.me/529930000000',
    path: 'M17.472 14.382c-.297-.149-1.758-.867-2.03-.967-.273-.099-.471-.148-.67.15-.197.297-.767.966-.94 1.164-.173.199-.347.223-.644.075-.297-.15-1.255-.463-2.39-1.475-.883-.788-1.48-1.761-1.653-2.059-.173-.297-.018-.458.13-.606.134-.133.298-.347.446-.52.149-.174.198-.298.298-.497.099-.198.05-.371-.025-.52-.075-.149-.669-1.612-.916-2.207-.242-.579-.487-.5-.669-.51-.173-.008-.371-.01-.57-.01-.198 0-.52.074-.792.372-.272.297-1.04 1.016-1.04 2.479 0 1.462 1.065 2.875 1.213 3.074.149.198 2.096 3.2 5.077 4.487.709.306 1.262.489 1.694.625.712.227 1.36.195 1.871.118.571-.085 1.758-.719 2.006-1.413.248-.694.248-1.289.173-1.413-.074-.124-.272-.198-.57-.347m-5.421 7.403h-.004a9.87 9.87 0 01-5.031-1.378l-.361-.214-3.741.982.998-3.648-.235-.374a9.86 9.86 0 01-1.51-5.26c.001-5.45 4.436-9.884 9.888-9.884 2.64 0 5.122 1.03 6.988 2.898a9.825 9.825 0 012.893 6.994c-.003 5.45-4.437 9.884-9.885 9.884m8.413-18.297A11.815 11.815 0 0012.05 0C5.495 0 .16 5.335.157 11.892c0 2.096.547 4.142 1.588 5.945L.057 24l6.305-1.654a11.882 11.882 0 005.683 1.448h.005c6.554 0 11.89-5.335 11.893-11.893a11.821 11.821 0 00-3.48-8.413z',
  },
]

const scrollTo = (href) => {
  const el = document.querySelector(href)
  if (el) el.scrollIntoView({ behavior: 'smooth' })
}
</script>

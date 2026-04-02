<template>
  <section
    id="redes"
    class="relative py-24 bg-gradient-to-br from-[#007FE1] via-[#00BFFF] to-[#00E0A6] overflow-hidden"
  >
    <!-- Overlay -->
    <div class="absolute inset-0 bg-black/40"></div>

    <div class="relative max-w-7xl mx-auto px-6">

      <!-- Header -->
      <div class="text-center mb-16">
        <span class="inline-block text-white text-xs tracking-[0.4em] uppercase font-semibold mb-3">
          Síguenos
        </span>

        <h2 class="text-4xl md:text-5xl font-bold text-white mb-4">
          Redes Sociales
        </h2>

        <div class="w-16 h-0.5 bg-[#C9A84C] mx-auto"></div>

        <p class="text-white/70 mt-6 max-w-lg mx-auto">
          Mantente conectado con nuestra comunidad. Compartimos mensajes de fe, eventos y transmisiones en vivo.
        </p>
      </div>

      <!-- Cards -->
      <div class="grid sm:grid-cols-2 lg:grid-cols-4 gap-6 mb-12">
        <a
          v-for="social in socials"
          :key="social.name"
          :href="social.url"
          target="_blank"
          rel="noopener noreferrer"
          class="group flex flex-col items-center text-center p-8 rounded-2xl border border-white/20 bg-white/10 backdrop-blur-md hover:bg-white/20 transition-all duration-300"
        >
          <div
            class="w-16 h-16 rounded-2xl flex items-center justify-center mb-5 transition-transform duration-300 group-hover:scale-110"
            :style="{ backgroundColor: social.color + '20', border: `1.5px solid ${social.color}40` }"
          >
            <component :is="social.iconComponent" :color="social.color" class="w-8 h-8" />
          </div>

          <h3 class="font-bold text-white mb-1 text-lg">
            {{ social.name }}
          </h3>

          <p class="text-white/60 text-xs mb-4 tracking-wider">
            {{ social.handle }}
          </p>

          <p class="text-white/70 text-sm leading-relaxed">
            {{ social.description }}
          </p>

          <span
            class="mt-5 text-xs font-semibold tracking-widest uppercase transition-all duration-300 group-hover:tracking-[0.2em]"
            :style="{ color: social.color }"
          >
            Seguir →
          </span>
        </a>
      </div>

      <!-- Live -->
      <div class="rounded-2xl border border-white/20 bg-white/10 backdrop-blur-md p-8 flex flex-col md:flex-row items-center gap-6 justify-between">
        <div class="flex items-center gap-4">
          <div class="w-12 h-12 rounded-full bg-red-500/20 border border-red-500/40 flex items-center justify-center">
            <span class="w-3 h-3 rounded-full bg-red-500 animate-pulse block"></span>
          </div>

          <div>
            <p class="text-white font-bold text-lg">
              Transmisiones en Vivo
            </p>
            <p class="text-white/60 text-sm">
              Domingos a las 10:00 AM por YouTube y Facebook
            </p>
          </div>
        </div>

        <a
          href="https://youtube.com"
          target="_blank"
          class="px-7 py-3 bg-[#C9A84C] text-[#1E293B] font-bold text-sm tracking-widest uppercase rounded-lg hover:bg-[#b8943e] transition-all whitespace-nowrap shadow-md"
        >
          Ver en Vivo
        </a>
      </div>
    </div>
  </section>
</template>

<script setup>
import { h } from 'vue'

// ICONOS (igual que ya tienes)
const FacebookIcon = ({ color }) => h('svg', { viewBox: '0 0 24 24', fill: color, class: 'w-8 h-8' }, [
  h('path', { d: 'M18 2h-3a5 5 0 00-5 5v3H7v4h3v8h4v-8h3l1-4h-4V7a1 1 0 011-1h3z' })
])

const InstagramIcon = ({ color }) => h('svg', { viewBox: '0 0 24 24', fill: 'none', stroke: color, 'stroke-width': '2', class: 'w-8 h-8' }, [
  h('rect', { x: '2', y: '2', width: '20', height: '20', rx: '5', ry: '5' }),
  h('path', { d: 'M16 11.37A4 4 0 1112.63 8 4 4 0 0116 11.37z' }),
  h('line', { x1: '17.5', y1: '6.5', x2: '17.51', y2: '6.5' })
])

const YoutubeIcon = ({ color }) => h('svg', { viewBox: '0 0 24 24', fill: color, class: 'w-8 h-8' }, [
  h('path', { d: 'M22.54 6.42a2.78 2.78 0 00-1.95-1.96C18.88 4 12 4 12 4s-6.88 0-8.59.46a2.78 2.78 0 00-1.95 1.96A29 29 0 001 12a29 29 0 00.46 5.58A2.78 2.78 0 003.41 19.6C5.12 20 12 20 12 20s6.88 0 8.59-.46a2.78 2.78 0 001.95-1.95A29 29 0 0023 12a29 29 0 00-.46-5.58z' }),
  h('polygon', { points: '9.75 15.02 15.5 12 9.75 8.98 9.75 15.02', fill: 'white' })
])

const WhatsappIcon = ({ color }) => h('svg', { viewBox: '0 0 24 24', fill: color, class: 'w-8 h-8' }, [
  h('path', { d: 'M17.472 14.382c-.297-.149-1.758-.867-2.03-.967-.273-.099-.471-.148-.67.15-.197.297-.767.966-.94 1.164-.173.199-.347.223-.644.075-.297-.15-1.255-.463-2.39-1.475-.883-.788-1.48-1.761-1.653-2.059-.173-.297-.018-.458.13-.606.134-.133.298-.347.446-.52.149-.174.198-.298.298-.497.099-.198.05-.371-.025-.52-.075-.149-.669-1.612-.916-2.207-.242-.579-.487-.5-.669-.51-.173-.008-.371-.01-.57-.01-.198 0-.52.074-.792.372-.272.297-1.04 1.016-1.04 2.479 0 1.462 1.065 2.875 1.213 3.074.149.198 2.096 3.2 5.077 4.487...' })
])

const socials = [
  {
    name: 'Facebook',
    handle: '@iglesiajesucristo',
    url: 'https://facebook.com',
    color: '#1877F2',
    description: 'Síguenos para mensajes de fe, eventos especiales y transmisiones en vivo.',
    iconComponent: FacebookIcon,
  },
  {
    name: 'Instagram',
    handle: '@jesucristo_sur',
    url: 'https://instagram.com',
    color: '#E1306C',
    description: 'Imágenes de nuestros eventos, devocionales diarios y momentos especiales.',
    iconComponent: InstagramIcon,
  },
  {
    name: 'YouTube',
    handle: 'Jesucristo Vida Eterna Sur',
    url: 'https://youtube.com',
    color: '#FF0000',
    description: 'Todos nuestros sermones, cultos en vivo y estudios bíblicos grabados.',
    iconComponent: YoutubeIcon,
  },
  {
    name: 'WhatsApp',
    handle: '+52 993 000 0000',
    url: 'https://wa.me/529930000000',
    color: '#25D366',
    description: 'Contáctanos directamente o únete a nuestros grupos de oración y avisos.',
    iconComponent: WhatsappIcon,
  },
]
</script>
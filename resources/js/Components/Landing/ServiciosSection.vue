<template>
  <section id="servicios" class="py-24 bg-[#1A2744] relative overflow-hidden">
    <!-- Background pattern -->
    <div class="absolute inset-0 opacity-5" style="background-image: radial-gradient(circle, #C9A84C 1px, transparent 1px); background-size: 40px 40px;"></div>

    <div class="relative z-10 max-w-7xl mx-auto px-6">

      <!-- Header -->
      <div class="text-center mb-16">
        <span class="inline-block text-[#C9A84C] text-xs tracking-[0.4em] uppercase font-semibold mb-3">Únete a Nosotros</span>
        <h2 class="text-4xl md:text-5xl font-bold text-white mb-4">Servicios & Horarios</h2>
        <div class="w-16 h-0.5 bg-[#C9A84C] mx-auto"></div>
        <p class="text-white/50 mt-6 max-w-xl mx-auto">Te invitamos a ser parte de nuestras reuniones semanales. Cada servicio es una oportunidad para encontrarte con Dios.</p>
      </div>

      <!-- Cards grid -->
      <div class="grid sm:grid-cols-2 lg:grid-cols-3 gap-6 mb-16">
        <div
          v-for="(service, index) in services"
          :key="service.name"
          class="group relative rounded-xl border border-white/10 p-7 hover:border-[#C9A84C]/50 transition-all duration-400 hover:bg-white/5 cursor-default"
        >
          <!-- Accent line top -->
          <div class="absolute top-0 left-7 right-7 h-0.5 bg-gradient-to-r from-transparent via-[#C9A84C] to-transparent opacity-0 group-hover:opacity-100 transition-opacity duration-400"></div>

          <div class="flex items-start justify-between mb-5">
            <div class="w-12 h-12 rounded-lg bg-[#C9A84C]/10 border border-[#C9A84C]/20 flex items-center justify-center text-2xl">
              {{ service.icon }}
            </div>
            <span class="text-[#C9A84C]/60 text-xs tracking-widest uppercase font-medium">{{ service.tag }}</span>
          </div>

          <h3 class="text-white font-bold text-lg mb-1">{{ service.name }}</h3>
          <p class="text-[#C9A84C] text-sm font-semibold mb-3">{{ service.day }} — {{ service.time }}</p>
          <p class="text-white/50 text-sm leading-relaxed">{{ service.description }}</p>
        </div>
      </div>

      <!-- Highlighted announcement -->
      <div class="rounded-2xl border border-[#C9A84C]/30 bg-[#C9A84C]/5 p-8 text-center max-w-2xl mx-auto">
        <div class="w-14 h-14 rounded-full border-2 border-[#C9A84C] flex items-center justify-center mx-auto mb-4">
          <svg class="w-6 h-6 text-[#C9A84C]" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 17h5l-1.405-1.405A2.032 2.032 0 0118 14.158V11a6.002 6.002 0 00-4-5.659V5a2 2 0 10-4 0v.341C7.67 6.165 6 8.388 6 11v3.159c0 .538-.214 1.055-.595 1.436L4 17h5m6 0v1a3 3 0 11-6 0v-1m6 0H9"/>
          </svg>
        </div>
        <h4 class="text-white font-bold text-xl mb-2">¡Todos son bienvenidos!</h4>
        <p class="text-white/60 text-sm leading-relaxed">
          No importa tu historia ni tu pasado. Las puertas de nuestra iglesia están abiertas para ti y tu familia.
          Ven como eres y experimenta el amor de Dios.
        </p>
      </div>
    </div>
  </section>
</template>

<script setup>
const services = [
  {
    icon: '🙏',
    tag: 'Semanal',
    name: 'Martes de Oración',
    day: 'Martes',
    time: '6:00 PM',
    description: 'Un tiempo especial para buscar a Dios en oración como iglesia unida.',
  },
  {
    icon: '👥',
    tag: 'Semanal',
    name: 'Reunión de Líderes',
    day: 'Martes',
    time: '7:00 PM',
    description: 'Espacio de formación, dirección y crecimiento para los líderes de la iglesia.',
  },
  {
    icon: '🔥',
    tag: 'Semanal',
    name: 'Jóvenes',
    day: 'Sábado',
    time: '3:30 PM',
    description: 'Reunión dinámica para jóvenes con enseñanza, convivencia y crecimiento espiritual.',
  },
  {
    icon: '🎯',
    tag: 'Semanal',
    name: '23 y Más',
    day: 'Sábado',
    time: '4:30 PM',
    description: 'Grupo para mayores de 23 años enfocado en crecimiento personal y espiritual.',
  },
  {
    icon: '👶',
    tag: 'Dominical',
    name: 'Escuela Dominical',
    day: 'Domingo',
    time: '8:45 AM',
    description: 'Espacio para niños donde aprenden principios bíblicos de forma divertida.',
  },
  {
    icon: '⛪',
    tag: 'Principal',
    name: 'Culto Dominical',
    day: 'Domingo',
    time: '10:00 AM',
    description: 'Servicio principal de adoración y predicación para toda la familia.',
  },
]
</script>
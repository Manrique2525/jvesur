<template>
  <section
    id="inicio"
    class="relative min-h-screen flex items-center justify-center overflow-hidden bg-gradient-to-br from-[#007FE1] via-[#00BFFF] to-[#00E0A6]"
  >
    <!-- Overlay oscuro para contraste -->
    <div class="absolute inset-0 bg-black/40"></div>

    <!-- Background pattern -->
    <div class="absolute inset-0 opacity-10">
      <div
        class="absolute inset-0"
        style="background-image: repeating-linear-gradient(0deg, transparent, transparent 60px, rgba(255,255,255,0.15) 60px, rgba(255,255,255,0.15) 61px), repeating-linear-gradient(90deg, transparent, transparent 60px, rgba(255,255,255,0.15) 60px, rgba(255,255,255,0.15) 61px);"
      ></div>
    </div>

    <!-- Glow moderno -->
    <div class="absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 w-[700px] h-[700px] rounded-full bg-white/10 blur-3xl pointer-events-none"></div>

    <!-- Cross decorativo -->
    <div class="absolute right-16 top-24 opacity-10">
      <svg width="180" height="260" viewBox="0 0 60 90" fill="none">
        <rect x="22" y="0" width="16" height="90" rx="3" fill="white"/>
        <rect x="0" y="22" width="60" height="16" rx="3" fill="white"/>
      </svg>
    </div>

    <div class="absolute left-10 bottom-20 opacity-5">
      <svg width="120" height="180" viewBox="0 0 60 90" fill="none">
        <rect x="22" y="0" width="16" height="90" rx="3" fill="white"/>
        <rect x="0" y="22" width="60" height="16" rx="3" fill="white"/>
      </svg>
    </div>

    <!-- Content -->
    <div class="relative z-10 text-center px-6 max-w-4xl mx-auto">
      <!-- Tag -->
      <div class="inline-flex items-center gap-2 mb-6 px-4 py-1.5 border border-white/30 rounded-full backdrop-blur-md bg-white/10">
        <span class="w-1.5 h-1.5 rounded-full bg-white animate-pulse"></span>
        <span class="text-white text-xs tracking-[0.3em] uppercase font-medium">
          Bienvenido a nuestra comunidad
        </span>
      </div>

      <!-- Title -->
      <h1 class="text-5xl md:text-7xl font-bold text-white leading-tight mb-4 animate-fade-up">
        Jesucristo<br>
        <span class="text-[#C9A84C]">Es La Vida Eterna</span>
      </h1>

      <p class="text-white/70 text-lg md:text-xl tracking-widest uppercase mb-8 font-light">
        — Sur —
      </p>

      <!-- Versículo -->
      <div class="max-w-xl mx-auto mb-10 p-5 border-l-2 border-[#C9A84C]/70 bg-white/10 backdrop-blur-md rounded-r-xl text-left">
        <p class="text-white/90 text-base italic leading-relaxed">
          "Yo soy la resurrección y la vida; el que cree en mí, aunque esté muerto, vivirá."
        </p>
        <p class="text-[#C9A84C] text-xs tracking-widest mt-2 uppercase font-semibold">
          — Juan 11:25
        </p>
      </div>

      <!-- Botones -->
      <div class="flex flex-col sm:flex-row gap-4 justify-center items-center">
        <a
          href="#nosotros"
          @click.prevent="scrollTo('#nosotros')"
          class="px-8 py-3.5 bg-[#C9A84C] text-[#1E293B] font-bold tracking-widest text-sm uppercase rounded hover:bg-[#b8943e] transition-all duration-300 shadow-lg"
        >
          Conócenos
        </a>

        <a
          href="#servicios"
          @click.prevent="scrollTo('#servicios')"
          class="px-8 py-3.5 border border-white/40 text-white font-medium tracking-widest text-sm uppercase rounded hover:border-[#C9A84C] hover:text-[#C9A84C] transition-all duration-300 backdrop-blur-sm"
        >
          Nuestros Horarios
        </a>
      </div>
    </div>

    <!-- Scroll -->
    <div class="absolute bottom-8 left-1/2 -translate-x-1/2 flex flex-col items-center gap-2 animate-bounce">
      <span class="text-white/50 text-xs tracking-widest uppercase">Descubre más</span>
      <svg class="w-4 h-4 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24">
        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 9l-7 7-7-7"/>
      </svg>
    </div>
  </section>
</template>

<script setup>
const scrollTo = (href) => {
  const el = document.querySelector(href)
  if (el) el.scrollIntoView({ behavior: 'smooth' })
}
</script>

<style scoped>
@keyframes fade-up {
  from { opacity: 0; transform: translateY(30px); }
  to   { opacity: 1; transform: translateY(0); }
}
.animate-fade-up {
  animation: fade-up 0.9s ease forwards;
}
</style>
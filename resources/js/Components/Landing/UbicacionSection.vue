<template>
  <section
    id="ubicacion"
    class="relative py-24 bg-gradient-to-br from-[#007FE1] via-[#00BFFF] to-[#00E0A6] overflow-hidden"
  >
    <!-- Overlay -->
    <div class="absolute inset-0 bg-black/40"></div>

    <div class="relative max-w-7xl mx-auto px-6">

      <!-- Header -->
      <div class="text-center mb-16">
        <span class="inline-block text-white text-xs tracking-[0.4em] uppercase font-semibold mb-3">
          Encuéntranos
        </span>

        <h2 class="text-4xl md:text-5xl font-bold text-white mb-4">
          ¿Dónde Estamos?
        </h2>

        <div class="w-16 h-0.5 bg-[#C9A84C] mx-auto"></div>
      </div>

      <div class="grid lg:grid-cols-5 gap-10 items-start">

        <!-- Info -->
        <div class="lg:col-span-2 space-y-6">
          <div class="bg-white/10 backdrop-blur-md border border-white/20 rounded-2xl p-7">

            <h3 class="text-white font-bold text-xl mb-5 pb-4 border-b border-white/20">
              Iglesia Cristiana Bautista Jesucristo Es La Vida Eterna
            </h3>

            <div class="space-y-4">

              <!-- Dirección -->
              <div class="flex items-start gap-3">
                <div class="w-9 h-9 min-w-[2.25rem] rounded-lg bg-white/10 border border-white/20 flex items-center justify-center">
                  <span class="text-[#C9A84C] text-sm">📍</span>
                </div>
                <div>
                  <p class="text-white/60 text-xs uppercase tracking-wider mb-0.5">
                    Dirección
                  </p>
                  <p class="text-white text-sm leading-relaxed">
                    Carr. a Teapa s/n, Col. Plutarco Elías Calles,<br>
                    86193 Villahermosa, Tabasco
                  </p>
                </div>
              </div>

              <!-- Horarios -->
              <div class="flex items-start gap-3">
                <div class="w-9 h-9 min-w-[2.25rem] rounded-lg bg-white/10 border border-white/20 flex items-center justify-center">
                  <span class="text-[#C9A84C] text-sm">🕐</span>
                </div>

                <div class="w-full">
                  <p class="text-white/60 text-xs uppercase tracking-wider mb-1">
                    Horarios de Servicios
                  </p>

                  <div class="space-y-1">
                    <div class="flex justify-between text-sm">
                      <span class="text-white/70">Martes (Oración)</span>
                      <span class="text-white font-semibold">6:00 PM</span>
                    </div>

                    <div class="flex justify-between text-sm">
                      <span class="text-white/70">Martes (Líderes)</span>
                      <span class="text-white font-semibold">7:00 PM</span>
                    </div>

                    <div class="flex justify-between text-sm">
                      <span class="text-white/70">Sábado (Jóvenes)</span>
                      <span class="text-white font-semibold">3:30 PM</span>
                    </div>

                    <div class="flex justify-between text-sm">
                      <span class="text-white/70">Sábado (23 y más)</span>
                      <span class="text-white font-semibold">4:30 PM</span>
                    </div>

                    <div class="flex justify-between text-sm">
                      <span class="text-white/70">Domingo (Escuela)</span>
                      <span class="text-white font-semibold">8:45 AM</span>
                    </div>

                    <div class="flex justify-between text-sm">
                      <span class="text-white/70">Domingo (Culto)</span>
                      <span class="text-white font-semibold">10:00 AM</span>
                    </div>
                  </div>
                </div>
              </div>

              <!-- Cómo llegar -->
              <div class="flex items-start gap-3">
                <div class="w-9 h-9 min-w-[2.25rem] rounded-lg bg-white/10 border border-white/20 flex items-center justify-center">
                  <span class="text-[#C9A84C] text-sm">🚌</span>
                </div>
                <div>
                  <p class="text-white/60 text-xs uppercase tracking-wider mb-0.5">
                    Cómo llegar
                  </p>
                  <p class="text-white/70 text-sm leading-relaxed">
                    Ubicados sobre la carretera a Teapa, en la colonia Plutarco Elías Calles. Acceso fácil en transporte público y vehículo.
                  </p>
                </div>
              </div>

            </div>

            <!-- Botón -->
            <a
              href="https://www.google.com/maps?q=17.968707893741442,-92.92231859934088"
              target="_blank"
              rel="noopener noreferrer"
              class="mt-6 flex items-center justify-center gap-2 w-full py-3 bg-[#C9A84C] text-[#1E293B] font-bold text-sm tracking-widest uppercase rounded-lg hover:bg-[#b8943e] transition-all shadow-md"
            >
              <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17.657 16.657L13.414 20.9a1.998 1.998 0 01-2.827 0l-4.244-4.243a8 8 0 1111.314 0z"/>
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 11a3 3 0 11-6 0 3 3 0 016 0z"/>
              </svg>
              Abrir en Google Maps
            </a>

          </div>
        </div>

        <!-- Mapa -->
        <div class="lg:col-span-3">
          <div class="rounded-2xl overflow-hidden border border-white/20 aspect-[4/3] w-full bg-white/10 backdrop-blur-md relative shadow-lg">
            <iframe
              src="https://www.google.com/maps?q=17.968707893741442,-92.92231859934088&hl=es&z=16&output=embed"
              width="100%"
              height="100%"
              style="border:0;"
              loading="lazy"
              class="absolute inset-0 w-full h-full"
            ></iframe>
          </div>
        </div>

      </div>
    </div>
  </section>
</template>
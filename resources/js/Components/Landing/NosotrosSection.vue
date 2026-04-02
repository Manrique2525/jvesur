<template>
    <section
        id="nosotros"
        class="relative py-24 bg-gradient-to-br from-[#007FE1] via-[#00BFFF] to-[#00E0A6] overflow-hidden"
    >
        <!-- Overlay para contraste -->
        <div class="absolute inset-0 bg-black/40"></div>

        <div class="relative max-w-7xl mx-auto px-6">
            <!-- Header -->
            <div class="text-center mb-16">
                <span
                    class="inline-block text-white text-xs tracking-[0.4em] uppercase font-semibold mb-3"
                >
                    Quiénes Somos
                </span>

                <h2 class="text-4xl md:text-5xl font-bold text-white mb-4">
                    Nuestra Iglesia
                </h2>

                <div class="w-16 h-0.5 bg-[#C9A84C] mx-auto"></div>
            </div>

            <div class="grid md:grid-cols-2 gap-16 items-center">
                <!-- Imagen -->
                <div class="relative">
                    <div
                        class="absolute -top-6 -left-6 w-48 h-48 border border-white/20 rounded-lg"
                    ></div>

                    <div
                        class="relative rounded-xl overflow-hidden aspect-[4/5] flex items-center justify-center bg-white/10 backdrop-blur-md border border-white/20"
                    >
                        <img
                            src="/images/Logo_Pulpito.png"
                            alt="Iglesia Jesucristo es la Vida Eterna Sur"
                            class="w-full h-full object-contain p-6"
                        />

                        <!-- Stats -->
                        <div
                            class="absolute bottom-4 left-4 right-4 flex gap-3"
                        >
                            <div
                                class="flex-1 bg-white/10 backdrop-blur-md border border-white/20 rounded-lg p-3 text-center"
                            >
                                <p class="text-[#C9A84C] text-2xl font-bold">
                                    +500
                                </p>
                                <p class="text-white/70 text-xs uppercase">
                                    Familias
                                </p>
                            </div>

                            <div
                                class="flex-1 bg-white/10 backdrop-blur-md border border-white/20 rounded-lg p-3 text-center"
                            >
                                <p class="text-[#C9A84C] text-2xl font-bold">
                                    15+
                                </p>
                                <p class="text-white/70 text-xs uppercase">
                                    Años
                                </p>
                            </div>

                            <div
                                class="flex-1 bg-white/10 backdrop-blur-md border border-white/20 rounded-lg p-3 text-center"
                            >
                                <p class="text-[#C9A84C] text-2xl font-bold">
                                    7
                                </p>
                                <p class="text-white/70 text-xs uppercase">
                                    Ministerios
                                </p>
                            </div>
                        </div>
                    </div>

                    <div
                        class="absolute -bottom-6 -right-6 w-32 h-32 border border-white/20 rounded-lg"
                    ></div>
                </div>

                <!-- Texto -->
                <div class="space-y-8">
                    <div>
                        <h3 class="text-2xl font-bold text-white mb-3">
                            Una comunidad de fe y amor
                        </h3>

                        <p class="text-white/80 leading-relaxed">
                            Somos una iglesia cristiana comprometida con
                            compartir el mensaje de salvación de Jesucristo.
                            Creemos que la fe transforma vidas, familias y
                            comunidades enteras. Nuestra misión es guiar a cada
                            persona hacia un encuentro genuino con Dios.
                        </p>
                    </div>

                    <!-- Valores -->
                    <div class="space-y-5">
                        <div
                            v-for="value in values"
                            :key="value.title"
                            class="flex items-start gap-4"
                        >
                            <div
                                class="w-10 h-10 min-w-[2.5rem] rounded-lg bg-white/10 backdrop-blur-md flex items-center justify-center border border-white/20"
                            >
                                <span class="text-[#C9A84C] text-lg">
                                    {{ value.icon }}
                                </span>
                            </div>

                            <div>
                                <h4 class="font-bold text-white mb-0.5">
                                    {{ value.title }}
                                </h4>

                                <p class="text-white/70 text-sm leading-relaxed">
                                    {{ value.description }}
                                </p>
                            </div>
                        </div>
                    </div>

                    <!-- CTA -->
                    <a
                        href="#servicios"
                        @click.prevent="scrollTo('#servicios')"
                        class="inline-flex items-center gap-2 text-[#C9A84C] font-semibold text-sm tracking-wider uppercase hover:gap-4 transition-all duration-300"
                    >
                        Ver nuestros servicios

                        <svg
                            class="w-4 h-4"
                            fill="none"
                            stroke="currentColor"
                            viewBox="0 0 24 24"
                        >
                            <path
                                stroke-linecap="round"
                                stroke-linejoin="round"
                                stroke-width="2"
                                d="M17 8l4 4m0 0l-4 4m4-4H3"
                            />
                        </svg>
                    </a>
                </div>
            </div>
        </div>
    </section>
</template>

<script setup>
const values = [
    {
        icon: "✝",
        title: "Fe en Cristo",
        description:
            "Fundamentamos todo en la Palabra de Dios y en la fe en Jesucristo como Señor y Salvador.",
    },
    {
        icon: "🤝",
        title: "Comunidad y Fraternidad",
        description:
            "Somos una familia que se apoya mutuamente en oración, amor y servicio desinteresado.",
    },
    {
        icon: "🕊️",
        title: "Discipulado",
        description:
            "Formamos creyentes comprometidos que crecen espiritualmente y sirven con propósito.",
    },
];

const scrollTo = (href) => {
    const el = document.querySelector(href);
    if (el) el.scrollIntoView({ behavior: "smooth" });
};
</script>
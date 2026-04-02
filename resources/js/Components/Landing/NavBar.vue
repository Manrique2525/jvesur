<template>
    <nav
        class="fixed top-0 left-0 w-full z-50 transition-all duration-500"
        :class="
            scrolled ? 'bg-[#1A2744] shadow-lg py-3' : 'bg-transparent py-5'
        "
    >
        <div class="max-w-7xl mx-auto px-6 flex items-center justify-between">
            <!-- Logo -->
            <div class="flex items-center gap-3">
                <img
                    src="/images/Logo_Pulpito.png"
                    alt="Logo Jesucristo es la Vida Eterna Sur"
                    class="w-10 h-10 rounded-full object-contain"
                />
                <div class="leading-tight">
                    <p
                        class="text-[#C9A84C] font-bold text-sm tracking-widest uppercase"
                    >
                        Jesucristo
                    </p>
                    <p class="text-white text-xs tracking-wider opacity-80">
                        Es La Vida Eterna Sur
                    </p>
                </div>
            </div>
            <!-- Desktop Menu -->
            <ul class="hidden md:flex items-center gap-8">
                <li v-for="item in menuItems" :key="item.id">
                    <a
                        :href="item.href"
                        class="text-sm tracking-wider uppercase transition-colors duration-300 hover:text-[#C9A84C]"
                        :class="scrolled ? 'text-white' : 'text-white'"
                        @click.prevent="scrollTo(item.href)"
                        >{{ item.label }}</a
                    >
                </li>
                <li>
                    <a
                        href="#contacto"
                        @click.prevent="scrollTo('#contacto')"
                        class="px-5 py-2 bg-[#C9A84C] text-[#1A2744] text-sm font-bold tracking-wider uppercase rounded hover:bg-[#b8943e] transition-colors duration-300"
                        >Contáctanos</a
                    >
                </li>
            </ul>

            <!-- Mobile Hamburger -->
            <button
                class="md:hidden text-white focus:outline-none"
                @click="mobileOpen = !mobileOpen"
                aria-label="Menú"
            >
                <svg
                    v-if="!mobileOpen"
                    class="w-7 h-7"
                    fill="none"
                    stroke="currentColor"
                    viewBox="0 0 24 24"
                >
                    <path
                        stroke-linecap="round"
                        stroke-linejoin="round"
                        stroke-width="2"
                        d="M4 6h16M4 12h16M4 18h16"
                    />
                </svg>
                <svg
                    v-else
                    class="w-7 h-7"
                    fill="none"
                    stroke="currentColor"
                    viewBox="0 0 24 24"
                >
                    <path
                        stroke-linecap="round"
                        stroke-linejoin="round"
                        stroke-width="2"
                        d="M6 18L18 6M6 6l12 12"
                    />
                </svg>
            </button>
        </div>

        <!-- Mobile Menu -->
        <transition name="slide-down">
            <div
                v-if="mobileOpen"
                class="md:hidden bg-[#1A2744] border-t border-[#C9A84C]/20 px-6 py-4"
            >
                <ul class="flex flex-col gap-4">
                    <li v-for="item in menuItems" :key="item.id">
                        <a
                            :href="item.href"
                            class="text-white text-sm tracking-wider uppercase hover:text-[#C9A84C] transition-colors block py-1"
                            @click.prevent="
                                scrollTo(item.href);
                                mobileOpen = false;
                            "
                            >{{ item.label }}</a
                        >
                    </li>
                    <li>
                        <a
                            href="#contacto"
                            @click.prevent="
                                scrollTo('#contacto');
                                mobileOpen = false;
                            "
                            class="inline-block px-5 py-2 bg-[#C9A84C] text-[#1A2744] text-sm font-bold tracking-wider uppercase rounded hover:bg-[#b8943e] transition-colors"
                            >Contáctanos</a
                        >
                    </li>
                </ul>
            </div>
        </transition>
    </nav>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from "vue";

const scrolled = ref(false);
const mobileOpen = ref(false);

const menuItems = [
    { id: 1, label: "Inicio", href: "#inicio" },
    { id: 2, label: "Nosotros", href: "#nosotros" },
    { id: 3, label: "Servicios", href: "#servicios" },
    { id: 4, label: "Ubicación", href: "#ubicacion" },
];

const handleScroll = () => {
    scrolled.value = window.scrollY > 50;
};

const scrollTo = (href) => {
    const el = document.querySelector(href);
    if (el) el.scrollIntoView({ behavior: "smooth" });
};

onMounted(() => window.addEventListener("scroll", handleScroll));
onUnmounted(() => window.removeEventListener("scroll", handleScroll));
</script>

<style scoped>
.slide-down-enter-active,
.slide-down-leave-active {
    transition: all 0.3s ease;
}
.slide-down-enter-from,
.slide-down-leave-to {
    opacity: 0;
    transform: translateY(-10px);
}
</style>

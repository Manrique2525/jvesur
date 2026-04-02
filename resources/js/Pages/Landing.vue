<template>
    <div class="antialiased">
        <NavBar />
        <HeroSection />
        <NosotrosSection />
        <ServiciosSection />
        <EventosSection />
        <GaleriaSection />
        <RedesSocialesSection />
        <ContactoSection />
        <UbicacionSection />
        <FooterSection />

        <!-- ═══════════════════════════════════════════════════ -->
        <!-- ELIGE UNO de los dos modales (no uses ambos):     -->
        <!-- ═══════════════════════════════════════════════════ -->

        <!-- OPCIÓN A: Modal con campos editables (título, fecha, invitado, etc.) -->
        <EventModal />

        <!-- OPCIÓN B: Modal con imagen/flyer del evento -->
        <!--  <EventImageModal /> -->

        <!-- ═══════════════════════════════════════════════════ -->
        <!-- CONEXIÓN CON BACKEND (cuando estés listo):         -->
        <!-- ═══════════════════════════════════════════════════ -->

        <!-- Opción A con datos del backend: -->
        <!-- <EventModal v-if="activeEvent" :event="activeEvent" /> -->

        <!-- Opción B con datos del backend: -->
        <!-- <EventImageModal v-if="activeImageModal" :modal="activeImageModal" /> -->
    </div>
</template>

<script setup>
import NavBar from '@/Components/Landing/NavBar.vue'
import HeroSection from '@/Components/Landing/HeroSection.vue'
import NosotrosSection from '@/Components/Landing/NosotrosSection.vue'
import ServiciosSection from '@/Components/Landing/ServiciosSection.vue'
import RedesSocialesSection from '@/Components/Landing/RedesSocialesSection.vue'
import ContactoSection from '@/Components/Landing/ContactoSection.vue'
import UbicacionSection from '@/Components/Landing/UbicacionSection.vue'
import FooterSection from '@/Components/Landing/FooterSection.vue'
// Descomenta el que uses:
import EventModal from '@/Components/Landing/EventModal.vue'
import EventImageModal from '@/Components/Landing/Eventimagemodal.vue'
import EventosSection from '@/Components/Landing/Eventossection.vue'
import GaleriaSection from '@/Components/Landing/GaleriaSection.vue'
</script>

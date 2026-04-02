<template>
  <div class="antialiased">
    <NavBar />
    <HeroSection />
    <NosotrosSection />
    <ServiciosSection />
    <RedesSocialesSection />
    <ContactoSection />
    <UbicacionSection />
    <FooterSection />
  </div>
</template>

<script setup>
import NavBar               from '@/Components/Landing/NavBar.vue'
import HeroSection          from '@/Components/Landing/HeroSection.vue'
import NosotrosSection      from '@/Components/Landing/NosotrosSection.vue'
import ServiciosSection     from '@/Components/Landing/ServiciosSection.vue'
import RedesSocialesSection from '@/Components/Landing/RedesSocialesSection.vue'
import ContactoSection      from '@/Components/Landing/ContactoSection.vue'
import UbicacionSection     from '@/Components/Landing/UbicacionSection.vue'
import FooterSection        from '@/Components/Landing/FooterSection.vue'
</script>
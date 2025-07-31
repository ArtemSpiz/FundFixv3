<script setup>
import "./style.css";
import Footer from "./layout/Footer/Footer.vue";
import HeaderLayout from "./layout/Header/HeaderLayout.vue";
import Hero from "./components/Hero/Hero.vue";
import Accessibility from "./components/Accessibility/Accessibility.vue";
import FIX from "./components/FIX/FIX.vue";
import Scale from "./components/Scale/Scale.vue";
import Future from "./components/Future/Future.vue";
</script>

<template>
  <HeaderLayout />
  <Hero />
  <Accessibility />
  <FIX />
  <Scale />
  <Future />
  <Footer />
</template>

<style scoped></style>

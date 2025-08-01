<script setup>
import "./style.css";
import HeaderLayout from "./layout/Header/HeaderLayout.vue";
import Hero from "./components/Hero/Hero.vue";
import Accessibility from "./components/Accessibility/Accessibility.vue";
import Scale from "./components/Scale/Scale.vue";
import Future from "./components/Future/Future.vue";
import Roadmap from "./components/Roadmap/Roadmap.vue";
import Exclusivity from "./components/Exclusivity/Exclusivity.vue";
import FIX from "./components/FIX/FIX.vue";
import FAQ from "./components/FAQ/FAQ.vue";
import Institutions from "./components/Institutions/Institutions.vue";
</script>

<template>
  <HeaderLayout />
  <Hero />
  <Accessibility />
  <FIX />
  <Scale />
  <Future />
  <Roadmap />
  <Exclusivity />
  <FAQ />
  <Institutions />
</template>

<style scoped></style>

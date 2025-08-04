<script setup>
import "./style.css";
import HeaderLayout from "./layout/Header/HeaderLayout.vue";
import Hero from "./components/Hero/Hero.vue";
import Accessibility from "./components/Accessibility/Accessibility.vue";
import Scale from "./components/Scale/Scale.vue";
import Future from "./components/Future/Future.vue";
import Roadmap from "./components/Roadmap/Roadmap.vue";
import FIX from "./components/FIX/FIX.vue";
import FAQ from "./components/FAQ/FAQ.vue";
import Institutions from "./components/Institutions/Institutions.vue";
import Footer from "./layout/Footer/Footer.vue";
import { ref } from "vue";
// import Tokenomics from "./components/Tokenomics/Tokenomics.vue";

const futureRef = ref(null);

function handleScrollToTokenomics() {
  futureRef.value?.scrollToTokenomics();
}
</script>

<template>
  <HeaderLayout :onScrollToTokenomics="handleScrollToTokenomics" />
  <Hero />
  <Accessibility />
  <FIX />
  <Scale />
  <Future />
  <!-- <Tokenomics /> -->
  <Roadmap />
  <FAQ />
  <Institutions />
  <Footer />
</template>

<style scoped></style>

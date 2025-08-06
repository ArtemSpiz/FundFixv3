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
import { ref, computed, onMounted } from "vue";
import gsap from "gsap";
import ScrollTrigger from "gsap/ScrollTrigger";

gsap.registerPlugin(ScrollTrigger);

const isOnRoadmapScroll = ref(false);
const isOnTokenomics = ref(false);

const isHeaderDark = computed(
  () => isOnRoadmapScroll.value || isOnTokenomics.value
);

onMounted(() => {
  ScrollTrigger.create({
    trigger: "#roadmap",
    start: "top center",
    end: "+=1000",
    scrub: true,
    onEnter: () => (isOnRoadmapScroll.value = true),
    onEnterBack: () => (isOnRoadmapScroll.value = true),
    onLeave: () => (isOnRoadmapScroll.value = false),
    onLeaveBack: () => (isOnRoadmapScroll.value = false),
  });

  const tokenomicsEl = document.querySelector("#tokenomics");
  if (!tokenomicsEl) return;

  const observer = new IntersectionObserver(
    ([entry]) => {
      isOnTokenomics.value = entry.isIntersecting;
    },
    { threshold: 0.1 }
  );

  observer.observe(tokenomicsEl);
});
</script>

<template>
  <HeaderLayout
    :onScrollToTokenomics="handleScrollToTokenomics"
    :isOnRoadmap="isHeaderDark"
  />
  <Hero />
  <!-- <Accessibility /> -->
  <FIX />
  <Scale />
  <Future />
  <!-- <Roadmap /> -->
  <FAQ />
  <Institutions />
</template>

<style scoped></style>

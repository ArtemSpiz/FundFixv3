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
import { ref, computed, onMounted, nextTick } from "vue";
import gsap from "gsap";
import ScrollTrigger from "gsap/ScrollTrigger";

gsap.registerPlugin(ScrollTrigger);

const isOnRoadmapScroll = ref(false);
const isOnTokenomics = ref(false);

const isHeaderDark = computed(
  () => isOnRoadmapScroll.value || isOnTokenomics.value
);

onMounted(async () => {
  await nextTick();

  setTimeout(() => {
    ScrollTrigger.create({
      trigger: "#roadmap",
      start: "top 80%",
      end: "bottom 50%", 
      onEnter: () => {
        console.log("Roadmap entered");
        isOnRoadmapScroll.value = true;
      },
      onEnterBack: () => {
        console.log("Roadmap entered back");
        isOnRoadmapScroll.value = true;
      },
      onLeave: () => {
        console.log("Roadmap left");
        isOnRoadmapScroll.value = false;
      },
      onLeaveBack: () => {
        console.log("Roadmap left back");
        isOnRoadmapScroll.value = false;
      },
    });

    const tokenomicsEl = document.querySelector("#tokenomics");
    if (tokenomicsEl) {
      const observer = new IntersectionObserver(
        ([entry]) => {
          isOnTokenomics.value = entry.isIntersecting;
        },
        { threshold: 0.1 }
      );

      observer.observe(tokenomicsEl);
    }
  }, 100);
});
</script>

<template>
  <HeaderLayout
    :onScrollToTokenomics="handleScrollToTokenomics"
    :isOnRoadmap="isHeaderDark"
  />
  <Hero />
  <Accessibility />
  <FIX />
  <Scale />
  <Future />
  <Roadmap />
  <FAQ />
  <Institutions />
</template>

<style scoped></style>

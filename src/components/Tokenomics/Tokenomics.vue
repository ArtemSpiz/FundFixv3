<script setup>
import { onMounted } from "vue";
import Stars from "@/assets/svg/Stars.vue";
import "./Tokenomics.css";
import TokenomicsPercents from "./TokenomicsPercents/TokenomicsPercents.vue";
import TokenomicsCards from "./TokenomicsCards/TokenomicsCards.vue";
import AnimatedText from "../AnimatedText.vue";

const isMobile = window.innerWidth < 769;

onMounted(() => {
  const observer = new IntersectionObserver(
    (entries) => {
      entries.forEach((entry) => {
        if (entry.isIntersecting) {
          entry.target.classList.add("show");
        } else {
          entry.target.classList.remove("show");
        }
      });
    },
    { threshold: isMobile ? 0.8 : 0.7 }
  );

  document.querySelectorAll(".animate-on-scroll").forEach((el) => {
    el.classList.add("hidden");
    observer.observe(el);
  });
});
</script>

<template>
  <div id="tokenomics" class="tokenomics">
    <div class="tokenomicsTexts animate-on-scroll">
      <div class="UnderTitle animate-on-scroll">
        <Stars />
        <AnimatedText text="Transparent Structure" />
      </div>
      <AnimatedText class="Title tokenomicsTitle" text="Our Tokenomics" />
    </div>

    <div class="animate-on-scroll">
      <TokenomicsPercents />
    </div>

    <div class="animate-on-scroll wrapperCardsToc">
      <TokenomicsCards />
    </div>
  </div>
</template>

<style scoped></style>

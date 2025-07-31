<script setup>
import { onMounted } from "vue";
import Stars from "@/assets/svg/Stars.vue";
import "./Tokenomics.css";
import TokenomicsPercents from "./TokenomicsPercents/TokenomicsPercents.vue";
import TokenomicsCards from "./TokenomicsCards/TokenomicsCards.vue";

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
    { threshold: 0.5 }
  );

  document.querySelectorAll(".animate-on-scroll").forEach((el) => {
    el.classList.add("hidden");
    observer.observe(el);
  });
});
</script>

<template>
  <div class="tokenomics">
    <div class="tokenomicsTexts">
      <div class="UnderTitle animate-on-scroll">
        <Stars /> Transparent Structure
      </div>
      <div class="Title tokenomicsTitle animate-on-scroll">Our Tokenomics</div>
    </div>

    <div class="animate-on-scroll">
      <TokenomicsPercents />
    </div>

    <div class="animate-on-scroll">
      <TokenomicsCards />
    </div>
  </div>
</template>

<style scoped></style>

<script setup>
import ExclusivityIcon1 from "@/assets/svg/ExclusivityIcon1.vue";
import ExclusivityIcon2 from "@/assets/svg/ExclusivityIcon2.vue";
import ExclusivityIcon3 from "@/assets/svg/ExclusivityIcon3.vue";
import ExclusivityIcon4 from "@/assets/svg/ExclusivityIcon4.vue";
import ExclusivityIcon5 from "@/assets/svg/ExclusivityIcon5.vue";
import ExclusivityIcon6 from "@/assets/svg/ExclusivityIcon6.vue";
import ExclusivityIcon7 from "@/assets/svg/ExclusivityIcon7.vue";
import Stars from "@/assets/svg/Stars.vue";
import "./Exclusivity.css";

const ExclusivityCards = [
  {
    icon: ExclusivityIcon1,
    title: "Accredited Investors",
  },
  {
    icon: ExclusivityIcon2,
    title: "Startup Operators",
  },
  {
    icon: ExclusivityIcon3,
    title: "Web3",
  },
  {
    icon: ExclusivityIcon4,
    title: "Native Capital",
  },
  {
    icon: ExclusivityIcon5,
    title: "LPs",
  },
  {
    icon: ExclusivityIcon6,
    title: "Managers",
  },
  {
    icon: ExclusivityIcon7,
    title: "Ecosystem Partners",
  },
];
</script>

<template>
  <div class="exclusivity">
    <div class="exclusivityTexts">
      <div class="exclusivityTitles">
        <div class="UnderTitle"><Stars />Exclusivity</div>
        <div class="exclusivityTitle Title">
          Not for Everyone. That’s the Point.
        </div>
      </div>

      <div class="Subtitle exclusivitySubtitle">
        FundFix is built for serious capital allocators —not hype-chasers.
      </div>
    </div>

    <div class="exclusivityCards">
      <div
        class="exclusivityCard"
        v-for="(card, index) in ExclusivityCards"
        :key="index"
        :class="`card-${index}`"
      >
        <component
          class="exclusivityCardIcon"
          :is="card.icon"
          :class="`cardIcon-${index}`"
        />
        <div class="exclusivityCardTitle" :class="`cardTitle-${index}`">
          {{ card.title }}
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped></style>

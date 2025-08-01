<script setup>
import Stars from "@/assets/svg/Stars.vue";
import institutionsLogoLeft from "@/assets/img/InstitutionsLeftLogo.png";
import institutionsLogoRight from "@/assets/img/InstitutionsRightLogo.png";
import Footer from "@/layout/Footer/Footer.vue";
import ArrowBtn from "@/assets/svg/ArrowBtn.vue";
import "./Institutions.css";
const title = "This Is What the Institutions Never Wanted You to Access";
const letters = title.split("");
</script>

<template>
  <div class="institutions">
    <div class="institutionsCard">
      <div class="institutionsLogoLeft">
        <img :src="institutionsLogoLeft" alt="" />
      </div>
      <div class="institutionsLogoRight">
        <img :src="institutionsLogoRight" alt="" />
      </div>

      <div class="institutionsTexts">
        <div class="institutionsTitles">
          <div class="UnderTitle institutionsUnderTitle">
            <Stars />Exclusivity
          </div>

          <div class="institutionsTitle animated-title">
            <span
              v-for="(letter, index) in letters"
              :key="index"
              class="letter"
              :style="{ animationDelay: `${index * 0.03}s` }"
            >
              {{ letter === " " ? "\u00A0" : letter }}
            </span>
          </div>
        </div>

        <div class="institutionsSubtitle Subtitle">
          FundFix isn’t here to democratize hype — it brings discipline,
          infrastructure, and upside to early movers.
        </div>
      </div>

      <button class="institutionsBtn">Join to FundFix <ArrowBtn /></button>
    </div>

    <Footer />
  </div>
</template>

<style scoped>
.animated-title {
  display: flex;
  flex-wrap: wrap;
  gap: 1px;
  overflow: hidden;
}

.letter {
  display: inline-block;
  opacity: 0;
  transform: translateY(20px);
  animation: riseFadeIn 0.4s forwards ease-out;
}

@keyframes riseFadeIn {
  to {
    opacity: 1;
    transform: translateY(0);
  }
}
</style>

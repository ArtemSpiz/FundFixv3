<script setup>
import Stars from "@/assets/svg/Stars.vue";
import institutionsLogoLeft from "@/assets/img/InstitutionsLeftLogo.png";
import institutionsLogoRight from "@/assets/img/InstitutionsRightLogo.png";
import Footer from "@/layout/Footer/Footer.vue";
import ArrowBtn from "@/assets/svg/ArrowBtn.vue";
import "./Institutions.css";
import AnimatedText from "@/components/AnimatedText.vue";
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
            <Stars color="#0f0f0f" />
            <AnimatedText text="Exclusivity" />
          </div>

          <AnimatedText
            class="institutionsTitle"
            text="This Is What the Institutions Never Wanted You to Access"
          />
        </div>

        <AnimatedText
          class="institutionsSubtitle Subtitle"
          text=" FundFix isn’t here to democratize hype — it brings discipline, infrastructure, and upside to early movers."
        />
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
  gap: 0.5px;
  overflow: hidden;
  justify-content: center;
  align-items: center;
}

.letter {
  display: inline-flex;
  opacity: 0;
  transform: translateY(20px);
}

.letter.visible {
  animation: riseFadeIn 0.4s forwards ease-out;
}

@keyframes riseFadeIn {
  to {
    opacity: 1;
    transform: translateY(0);
  }
}
</style>

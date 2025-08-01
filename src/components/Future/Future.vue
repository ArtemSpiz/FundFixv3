<script setup>
import Stars from "@/assets/svg/Stars.vue";
import phoneText from "@/assets/img/futurePhoneText.png";
import phoneCont from "@/assets/img/futurePhoneCont.png";
import phoneBorder from "@/assets/img/futurePhoneBg.png";

import "./Future.css";

const FutureCards = [
  {
    title: "Deal Diligence Process",
    text: "Every opportunity on FundFix goes through a multi-phase vetting pipeline",
    subtitles: [
      { subtitle: "Financial/legal audit" },
      { subtitle: "Market sizing & competitive analysis" },
      { subtitle: "Technical team & roadmap review" },
      { subtitle: "KPI and traction validation" },
    ],
  },
  {
    title: "Platform Transparency",
    text: "FundFix provides tools and insights for smarter investing",
    subtitles: [
      { subtitle: "Real companies with traction" },
      { subtitle: "Performance dashboards" },
      { subtitle: "Investment memos & pitch videos" },
    ],
  },
];

import { onMounted, onBeforeUnmount } from "vue";
import gsap from "gsap";
import ScrollTrigger from "gsap/ScrollTrigger";
import Tokenomics from "../Tokenomics/Tokenomics.vue";

gsap.registerPlugin(ScrollTrigger);

onMounted(() => {
  const cards = document.querySelectorAll(".futureCard");
  const phoneImage = document.querySelector(".futurePhone");
  const overlay = document.querySelector(".phoneOverlay");
  const futureTexts = document.querySelector(".futureTexts");
  const futureSection = document.querySelector(".future");
  const tokenomics = document.querySelector(".tokenomicsInFuture");

  // Адаптивні параметри
  const isMobile = window.innerWidth <= 768;
  const isTablet = window.innerWidth <= 1024;

  const initialWidth = phoneImage.offsetWidth;
  const targetWidth = window.innerWidth;

  // Менше масштабування телефону
  let scaleFactor;
  if (isMobile) {
    scaleFactor = (targetWidth / initialWidth) * 0.6; // Зменшено з 0.8
  } else if (isTablet) {
    scaleFactor = (targetWidth / initialWidth) * 0.7; // Зменшено з 0.9
  } else {
    scaleFactor = (targetWidth / initialWidth) * 1.1; // Зменшено з 1.1
  }

  const tl = gsap.timeline({
    scrollTrigger: {
      id: "future-sequence",
      trigger: futureSection,
      pin: futureSection,
      start: "top top",
      end: `+=${cards.length * 1000 + 800}`,
      scrub: 1.2,
      anticipatePin: 1,
    },
  });

  // Початкові налаштування для Tokenomics - ЗНАЧНО менший розмір
  gsap.set(tokenomics, {
    position: "absolute",
    top: "50%",
    left: "50%",
    xPercent: -50,
    yPercent: -50,
    opacity: 0,
    scale: isMobile ? 0.25 : isTablet ? 0.3 : 0.35, // ЗНАЧНО зменшено
    transformOrigin: "center center",
  });

  gsap.set(overlay, {
    scaleY: 0,
    transformOrigin: "bottom center",
    opacity: 1,
  });

  // Анімація карток і тексту
  tl.to([futureTexts, cards], {
    yPercent: -400,
    opacity: 0,
    ease: "power2.out",
    duration: 3,
  });

  // Менший рух телефону
  tl.to(
    phoneImage,
    {
      y: isMobile ? -50 : -200, // Зменшено рух
      ease: "power2.inOut",
      duration: 1,
    },
    "<+0.2"
  );

  // Поява overlay
  tl.to(
    overlay,
    {
      scaleY: 1,
      opacity: 1,
      duration: 2.5,
      ease: "power2.inOut",
    },
    "<+=0.3"
  );

  tl.to(
    phoneImage,
    {
      scale: scaleFactor,
      ease: "power2.inOut",
      duration: 3.5,
    },
    "<+=0.2"
  );

  tl.to(
    tokenomics,
    {
      opacity: 1,
      scale: isMobile ? 0.3 : isTablet ? 0.35 : 0.27,
      y: 20,
      ease: "power2.inOut",
      duration: 2,
    },
    "<+=1"
  );

  // Intersection Observer
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
    {
      threshold: 0.5,
      rootMargin: "0px 0px -50px 0px",
    }
  );

  const handleResize = () => {
    ScrollTrigger.refresh();
  };

  window.addEventListener("resize", handleResize);

  onBeforeUnmount(() => {
    window.removeEventListener("resize", handleResize);
    observer.disconnect();
  });
});

onBeforeUnmount(() => {
  ScrollTrigger.getById("future-sequence")?.kill();
  ScrollTrigger.refresh();
});
</script>

<template>
  <div class="future">
    <div class="futureTexts">
      <div class="UnderTitle"><Stars /> Next-Gen Investing</div>
      <div class="Title futureTitle">
        The Future of Private <span>Investing — Today</span>
      </div>
    </div>

    <div class="futurePhoneWrap">
      <div class="futurePhone">
        <div class="futurePhoneBorder">
          <img :src="phoneBorder" alt="" />
        </div>
        <div class="phoneContent">
          <div class="futurePhoneText">
            <img :src="phoneText" alt="" />
          </div>
          <img :src="phoneCont" alt="" />
        </div>

        <div class="phoneOverlay">
          <div class="tokenomicsWrapper">
            <Tokenomics class="tokenomicsInFuture" />
          </div>
        </div>
      </div>
    </div>

    <div
      class="futureCard"
      v-for="(card, index) in FutureCards"
      :key="index"
      :class="{ left: index === 0, right: index === 1 }"
    >
      <div class="futureCardTexts">
        <div class="futureCardTitle">{{ card.title }}</div>
        <div class="futureCardText">{{ card.text }}</div>
      </div>
      <div class="futureCardSubtitles">
        <div
          class="futureCardSubtitle"
          v-for="(sub, i) in card.subtitles"
          :key="i"
          :class="{
            firstSubFut: index === 0 && i === 0,
            secSubFut: index === 0 && i === 1,
            thirdSubFut: index === 0 && i === 2,
            fourthSubFut: index === 0 && i === 3,
            firstSubSecCardFut: index === 1 && i === 0,
            secSubSecCardFut: index === 1 && i === 1,
            thirdSubSecCardFut: index === 1 && i === 2,
          }"
        >
          {{ sub.subtitle }}
        </div>
      </div>
    </div>
  </div>
</template>

<style lang="scss" scoped></style>

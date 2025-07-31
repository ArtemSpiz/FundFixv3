<script setup>
import Stars from "@/assets/svg/Stars.vue";
import phone from "@/assets/img/iPhoneFuture.png";
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
  const phoneImage = document.querySelector(".futurePhone img");
  const overlay = document.querySelector(".phoneOverlay");

  const initialWidth = phoneImage.offsetWidth;
  const targetWidth = window.innerWidth;
  const scaleFactor = targetWidth / initialWidth + 0.2;
  const tl = gsap.timeline({
    scrollTrigger: {
      id: "future-sequence",
      trigger: ".futureTitle",
      pin: ".future",
      start: "top top",
      end: `+=${cards.length * 800 + 800}`,
      scrub: 1,
    },
  });

  tl.to(cards, {
    yPercent: -300,
    ease: "power3.out",
    duration: 4,
  });

  tl.fromTo(
    phoneImage,
    { scale: 1 },
    {
      scale: scaleFactor,
      transformOrigin: "center center",
      ease: "power3.inOut",
      duration: 2,
    },
    "-=3"
  );

  tl.fromTo(
    overlay,
    { scaleX: 0 },
    {
      scaleX: 1,
      duration: 4,
      ease: "power3.inOut",
    },
    "-=1.4"
  );

  tl.to(
    ".tokenomicsInFuture",
    {
      opacity: 1,
      y: 0,
      ease: "power2.out",
      duration: 1.2,
    },
    "-=0.5"
  );

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
});

onBeforeUnmount(() => {
  ScrollTrigger.getById("future-sequence")?.kill();
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

    <div class="futurePhone">
      <img :src="phone" alt="" />
      <div class="phoneOverlay">
        <Tokenomics class="tokenomicsInFuture animate-on-scroll" />
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

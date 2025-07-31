<script setup>
import CompMouse from "@/assets/svg/CompMouse.vue";
import "./Hero.css";
import ArrowBtn from "@/assets/svg/ArrowBtn.vue";

import heroCard1 from "@/assets/img/heroCard2.png";
import heroCard2 from "@/assets/img/heroCard2.png";
import heroCard3 from "@/assets/img/heroCard2.png";
import heroCard4 from "@/assets/img/heroCard2.png";
import heroCard5 from "@/assets/img/heroCard2.png";
import heroCard6 from "@/assets/img/heroCard2.png";
import heroCard7 from "@/assets/img/heroCard2.png";
import heroCardIcon from "@/assets/img/heroCardActive.png";
import { onMounted, onUnmounted, ref, watch } from "vue";

const HeroCards = [
  {
    image: heroCard1,
    icon: heroCardIcon,
    name: "Changpeng Zhao",
    position: "Binance founder",
  },
  {
    image: heroCard2,
    icon: heroCardIcon,
    name: "Changpeng Zhao",
    position: "Binance founder",
  },
  {
    image: heroCard3,
    icon: heroCardIcon,
    name: "Changpeng Zhao",
    position: "Binance founder",
  },
  {
    image: heroCard4,
    icon: heroCardIcon,
    name: "Changpeng Zhao",
    position: "Binance founder",
  },
  {
    image: heroCard5,
    icon: heroCardIcon,
    name: "Changpeng Zhao",
    position: "Binance founder",
  },
  {
    image: heroCard6,
    icon: heroCardIcon,
    name: "Changpeng Zhao",
    position: "Binance founder",
  },
  {
    image: heroCard7,
    icon: heroCardIcon,
    name: "Changpeng Zhao",
    position: "Binance founder",
  },

  {
    image: heroCard5,
    icon: heroCardIcon,
    name: "Changpeng Zhao",
    position: "Binance founder",
  },
  {
    image: heroCard6,
    icon: heroCardIcon,
    name: "Changpeng Zhao",
    position: "Binance founder",
  },
  {
    image: heroCard7,
    icon: heroCardIcon,
    name: "Changpeng Zhao",
    position: "Binance founder",
  },
];

const heroRef = ref(null);
const isVisible = ref(false);

const cardsWithPositions = ref([]);
const activeIndex = ref(0);

let intervalId = null;

function updatePositions() {
  const screenWidth = window.innerWidth;
  const screenHeight = window.innerHeight;
  const radius = screenWidth / 2.2;
  const centerX = screenWidth / 2;
  const centerY = screenHeight;

  const total = HeroCards.length;
  const visibleCount = 7;
  const centerIndex = Math.floor(visibleCount / 2);
  const maxRotationDeg = 90;

  const visibleCards = [];

  for (let i = 0; i < visibleCount; i++) {
    const realIndex = (activeIndex.value - centerIndex + i + total) % total;

    const angleStep = Math.PI / (visibleCount - 1);
    const angle = angleStep * i;

    const x = centerX + radius * Math.cos(angle);
    const y = centerY - radius * Math.sin(angle);

    const offsetFromCenter = i - centerIndex;
    const rotateDeg = (-maxRotationDeg * offsetFromCenter) / centerIndex;

    visibleCards.push({
      ...HeroCards[realIndex],
      originalIndex: realIndex,
      style: {
        left: `${x}px`,
        top: `${y}px`,
        position: "absolute",
        transition: "all 1.2s ease",
        transform: `translate(-50%, -50%) rotate(${rotateDeg}deg) rotateY(${
          offsetFromCenter === 0 ? 360 : 0
        }deg)`,
        zIndex: offsetFromCenter === 0 ? 10 : 1,
        opacity: offsetFromCenter === 0 ? 1 : 0.7,
      },
      rotateDeg,
      isActive: offsetFromCenter === 0,
    });
  }

  cardsWithPositions.value = visibleCards;
}

onMounted(() => {
  intervalId = setInterval(() => {
    activeIndex.value = (activeIndex.value - 1) % HeroCards.length;
  }, 5000);

  updatePositions();
});

onUnmounted(() => {
  clearInterval(intervalId);
});

watch(activeIndex, () => {
  updatePositions();
});

let observer = null;

function startAutoScroll() {
  if (!intervalId) {
    intervalId = setInterval(() => {
      activeIndex.value = (activeIndex.value - 1) % HeroCards.length;
    }, 5000);
  }
}

function stopAutoScroll() {
  clearInterval(intervalId);
  intervalId = null;
}

function handleVisibilityChange() {
  if (document.hidden) {
    stopAutoScroll();
  } else if (isVisible.value) {
    startAutoScroll();
  }
}

onMounted(() => {
  updatePositions();

  observer = new IntersectionObserver(
    ([entry]) => {
      isVisible.value = entry.isIntersecting;

      if (document.hidden) return;

      if (entry.isIntersecting) {
        startAutoScroll();
      } else {
        stopAutoScroll();
      }
    },
    {
      threshold: 0.5,
    }
  );

  if (heroRef.value) {
    observer.observe(heroRef.value);
  }

  document.addEventListener("visibilitychange", handleVisibilityChange);
});

onUnmounted(() => {
  stopAutoScroll();

  if (observer && heroRef.value) {
    observer.unobserve(heroRef.value);
  }

  document.removeEventListener("visibilitychange", handleVisibilityChange);
});
</script>

<template>
  <div class="hero" ref="heroRef">
    <div class="heroTexts">
      <div class="heroTitle">FundFix backs what’s next</div>
      <div class="heroSubtitle">
        FundFix opens up access to early investments through a tokenized
        platform.
      </div>

      <button class="heroBtn">Join to FundFix <ArrowBtn /></button>
    </div>
    <div class="heroBottom"><CompMouse /> Scoll to Discover</div>

    <div class="heroCards">
      <div
        class="heroCard"
        :class="{ active: card.isActive }"
        v-for="card in cardsWithPositions"
        :key="card.originalIndex"
        :style="card.style"
      >
        <div
          class="heroCardInner"
          :style="{ transform: `rotate(${-card.rotateDeg}deg)` }"
        >
          <div
            class="heroCardImg"
            :style="{ transform: `rotate(${card.rotateDeg}deg)` }"
          >
            <img :src="card.image" alt="" />
          </div>

          <div
            class="heroCardContent"
            :class="{ active: card.isActive }"
            :style="{ transform: `rotate(${card.rotateDeg}deg)` }"
          >
            <div class="heroCardContIcon">
              <img :src="card.icon" alt="" />
            </div>
            <div class="heroCardTexts">
              <div class="heroCardContTitle">{{ card.name }}</div>
              <div class="heroCardContSubtitle">{{ card.position }}</div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style lang="scss" scoped></style>

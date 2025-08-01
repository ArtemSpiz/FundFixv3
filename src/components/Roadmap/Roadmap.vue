<script setup>
import Arrow from "@/assets/svg/Arrow.vue";
import Stars from "@/assets/svg/Stars.vue";
import "./Roadmap.css";

const RoadmapCards = [
  {
    title: "Q1 2025",
    subtitle:
      "Team formation, regulatory onboarding, private investor roadshows ",
  },
  {
    title: "Q2 2025",
    subtitle:
      "Team formation, regulatory onboarding, private investor roadshows 2",
  },
  {
    title: "Q3 2025",
    subtitle:
      "Team formation, regulatory onboarding, private investor roadshows 3",
  },
  {
    title: "Q4 2025",
    subtitle:
      "Team formation, regulatory onboarding, private investor roadshows 4",
  },
  {
    title: "Q1 2026",
    subtitle:
      "Team formation, regulatory onboarding, private investor roadshows ",
  },
  {
    title: "Q2 2026",
    subtitle:
      "Team formation, regulatory onboarding, private investor roadshows ",
  },
  {
    title: "Q3 2026",
    subtitle:
      "Team formation, regulatory onboarding, private investor roadshows ",
  },
  {
    title: "Q4 2026",
    subtitle:
      "Team formation, regulatory onboarding, private investor roadshows ",
  },
  {
    title: "Q1 2027",
    subtitle:
      "Team formation, regulatory onboarding, private investor roadshows ",
  },
  {
    title: "Q2 2027",
    subtitle:
      "Team formation, regulatory onboarding, private investor roadshows ",
  },
];

import { ref, onMounted, computed } from "vue";
import AnimatedText from "../AnimatedText.vue";

const activeIndex = ref(0);
const pathLength = ref(0);

const prev = () => {
  if (activeIndex.value > 0) activeIndex.value--;
};

const next = () => {
  if (activeIndex.value < RoadmapCards.length - 1) activeIndex.value++;
};

onMounted(() => {
  const path = document.querySelector(".roadmapSVG path");
  if (path) {
    pathLength.value = path.getTotalLength();
  }
});

const activeStrokeStyle = computed(() => {
  const total = RoadmapCards.length;
  const segment = pathLength.value / (total - 1);
  const visibleLength =
    activeIndex.value === 0
      ? 70
      : activeIndex.value === 9
      ? segment * activeIndex.value - 70
      : segment * activeIndex.value;

  return {
    strokeDasharray: pathLength.value,
    strokeDashoffset: pathLength.value - visibleLength,
    transition: "stroke-dashoffset 0.6s ease-in-out",
  };
});

const getPointPosition = (index, total) => {
  const radius = 450;
  const centerX = 500;
  const centerY = 450;

  const angle = Math.PI * (1 - index / (total - 1));
  const x = centerX + radius * Math.cos(angle);
  let y = centerY - radius * Math.sin(angle);

  if (index === 0 || index === total - 1) {
    y -= 50;
  }

  return `position: absolute; left: ${x}px; top: ${y}px; transform: translate(-50%, -50%)`;
};

const getLineStyle = (index, total) => {
  const radius = 387;
  const centerX = 500;
  const centerY = 450;

  const angle = Math.PI * (1 - index / (total - 1));

  const x1 = centerX + radius * Math.cos(angle);
  let y1 = centerY - radius * Math.sin(angle);

  if (index === 0 || index === total - 1) {
    y1 -= 50;
  }

  const length = 20;
  const angleDeg = (angle * 180) / Math.PI;

  return `position: absolute; left: ${x1}px; top: ${y1}px; width: ${length}px; transform-origin: left center; transform: rotate(${-angleDeg}deg);`;
};
</script>

<template>
  <div class="roadmap">
    <div class="roadmapTexts">
      <div class="roadmapTitles">
        <div class="UnderTitle">
          <Stars />
          <AnimatedText text="What's Ahead" />
        </div>
        <AnimatedText class="Title roadmapTitle" text="Our Roadmap" />
      </div>

      <AnimatedText
        class="Subtitle roadmapSubtitle"
        text="Access the Deals Behind Silicon Valley's Greatest Success Stories—At the Speed of Blockchain"
      />
    </div>

    <div class="roadmapCards">
      <div class="roadmapArk">
        <svg
          class="roadmapSVG"
          width="1000"
          viewBox="0 0 1300 800"
          xmlns="http://www.w3.org/2000/svg"
          preserveAspectRatio="xMidYMid meet"
        >
          <path
            d="M 150 590 A 500 500 0 0 1 1150 590"
            stroke="#F1F1F1"
            stroke-width="20"
            fill="none"
            stroke-linecap="round"
          />

          <path
            ref="activePath"
            d="M 150 590 A 500 500 0 0 1 1150 590"
            stroke="#151515"
            stroke-width="20"
            fill="none"
            stroke-linecap="round"
            :style="activeStrokeStyle"
          />
        </svg>

        <div
          v-for="(item, index) in RoadmapCards"
          :key="index"
          class="roadmapPoint"
          :style="getPointPosition(index, RoadmapCards.length)"
          @click="activeIndex = index"
        >
          {{ item.title }}
        </div>

        <div
          v-for="(item, index) in RoadmapCards"
          :key="'line-' + index"
          class="roadmapLine"
          :style="getLineStyle(index, RoadmapCards.length)"
          @click="activeIndex = index"
          :class="{ active: activeIndex >= index }"
        />
      </div>
      <div class="roadmapCardsBottom">
        <div class="roadmapCardsTexts">
          <transition name="fade" mode="out-in">
            <div
              class="roadmapCardsTitle"
              :key="RoadmapCards[activeIndex].title"
            >
              {{ RoadmapCards[activeIndex].title }}
            </div>
          </transition>

          <transition name="fade" mode="out-in">
            <div
              class="roadmapCardsSubtitle"
              :key="RoadmapCards[activeIndex].subtitle"
            >
              {{ RoadmapCards[activeIndex].subtitle }}
            </div>
          </transition>
        </div>

        <div class="roadmapCardsBtns">
          <div class="roadmapCardsArrow left" @click="prev"><Arrow /></div>
          <div class="roadmapCardsArrow" @click="next"><Arrow /></div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped></style>

<script setup>
import TokenomicsIcon1 from "@/assets/svg/TokenomicsIcon1.vue";
import "./TokenomicsCards.css";
import TokenomicsIcon2 from "@/assets/svg/TokenomicsIcon2.vue";
import TokenomicsIcon3 from "@/assets/svg/TokenomicsIcon3.vue";
import TokenomicsIcon4 from "@/assets/svg/TokenomicsIcon4.vue";
import TokenomicsIcon5 from "@/assets/svg/TokenomicsIcon5.vue";
import BgTokCards from "@/assets/svg/bgTokCards.vue";
import Arrow from "@/assets/svg/Arrow.vue";
import { onMounted, onBeforeUnmount, ref } from "vue";

const TokenomicsCards = [
  {
    icon: TokenomicsIcon1,
    title: "Presale",
    subtitles: [{ subtitle: "TGE" }, { text: "20%" }],
    texts: [{ subtitle: "Over 12 month" }, { text: "80%" }],
  },
  {
    icon: TokenomicsIcon2,
    title: "Liquidity Pool",
    subtitles: [{ subtitle: "TGE" }, { text: "Full liquid%" }],
  },
  {
    icon: TokenomicsIcon3,
    title: "Team & Advisors",
    subtitles: [{ subtitle: "Month Cliff" }, { text: "6" }],
    texts: [{ subtitle: "Month vest" }, { text: "24" }],
  },
  {
    icon: TokenomicsIcon4,
    title: "Marketing",
    subtitles: [{ text: "Ongoing support" }],
  },
  {
    icon: TokenomicsIcon5,
    title: "Development Fund",
    subtitles: [{ subtitle: "Month Cliff" }, { text: "3" }],
    texts: [{ subtitle: "Month vest" }, { text: "18" }],
  },
];

let scrollTimeout = null;
const scrollContainer = ref(null);
let step = 0;

function updateStep() {
  if (!scrollContainer.value) return;
  const card = scrollContainer.value.querySelector(".tokenomicsCardWrapper");
  if (card) {
    const gap = 16;
    step = card.offsetWidth + gap;
  }
}

function alignToCard() {
  if (!scrollContainer.value || step === 0) return;

  const scrollLeft = scrollContainer.value.scrollLeft;
  const nearest = Math.round(scrollLeft / step) * step;

  scrollContainer.value.scrollTo({
    left: nearest,
    behavior: "smooth",
  });
}

function onScroll() {
  clearTimeout(scrollTimeout);
  scrollTimeout = setTimeout(() => {
    alignToCard();
  }, 150);
}

function scrollNext() {
  if (!scrollContainer.value || step === 0) return;

  scrollContainer.value.scrollBy({
    left: step,
    behavior: "smooth",
  });
}

function scrollPrev() {
  if (!scrollContainer.value || step === 0) return;

  scrollContainer.value.scrollBy({
    left: -step,
    behavior: "smooth",
  });
}

onMounted(() => {
  updateStep();
  if (scrollContainer.value) {
    scrollContainer.value.scrollLeft = 0;
    scrollContainer.value.addEventListener("scroll", onScroll);
  }
  window.addEventListener("resize", updateStep);
});

onBeforeUnmount(() => {
  if (scrollContainer.value) {
    scrollContainer.value.removeEventListener("scroll", onScroll);
  }
  window.removeEventListener("resize", updateStep);
});
</script>

<template>
  <div class="tokenomicsCardsWrapper animate-on-scroll">
    <div class="tokenomicsCards">
      <div
        class="tokenomicsCardsScroll animate-on-scroll"
        ref="scrollContainer"
      >
        <div
          class="tokenomicsCardWrapper"
          v-for="(card, index) in TokenomicsCards"
          :key="index"
          :class="{ centeredCards: index === 3, centeredCardsSec: index === 4 }"
        >
          <div class="tokenomicsCard">
            <BgTokCards class="BgTokCards" />
            <div class="tokenomicsCardTop">
              <component :is="card.icon" />
              <div class="tokenomicsCardTitle">{{ card.title }}</div>
            </div>

            <div class="tokenomicsCardBottom">
              <div class="tokenomicsCardSubtitlesWrapper">
                <div
                  class="tokenomicsCardSubtitles"
                  v-for="(sub, i) in card.subtitles"
                  :key="i"
                >
                  <div v-if="sub.subtitle" class="tokenomicsCardSubtitle">
                    {{ sub.subtitle }}
                  </div>
                  <div class="tokenomicsCardText">{{ sub.text }}</div>
                </div>
              </div>
              <div class="tokenomicsCardLine" v-if="card.texts" />

              <div class="tokenomicsCardSubtitlesWrapper">
                <template v-if="card.texts">
                  <div
                    class="tokenomicsCardSubtitles"
                    v-for="(text, q) in card.texts"
                    :key="q"
                  >
                    <div class="tokenomicsCardSubtitle">
                      {{ text.subtitle }}
                    </div>
                    <div class="tokenomicsCardText">{{ text.text }}</div>
                  </div>
                </template>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <div class="tokenomicsCardsBtns">
      <div class="tokenomicsCardsArrow left" @click="scrollPrev"><Arrow /></div>
      <div class="tokenomicsCardsArrow" @click="scrollNext"><Arrow /></div>
    </div>
  </div>
</template>

<style scoped></style>

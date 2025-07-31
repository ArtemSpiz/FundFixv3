<script setup>
import TokenomicsIcon1 from "@/assets/svg/TokenomicsIcon1.vue";
import "./TokenomicsCards.css";
import TokenomicsIcon2 from "@/assets/svg/TokenomicsIcon2.vue";
import TokenomicsIcon3 from "@/assets/svg/TokenomicsIcon3.vue";
import TokenomicsIcon4 from "@/assets/svg/TokenomicsIcon4.vue";
import TokenomicsIcon5 from "@/assets/svg/TokenomicsIcon5.vue";
import BgTokCards from "@/assets/svg/bgTokCards.vue";

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
</script>

<template>
  <div class="tokenomicsCards">
    <div
      class="tokenomicsCardWrapper"
      v-for="(card, index) in TokenomicsCards"
      :key="index"
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
</template>

<style scoped></style>

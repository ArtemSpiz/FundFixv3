<script setup>
import Stars from "@/assets/svg/Stars.vue";
import AnimatedText from "@/components/AnimatedText.vue";
import PointCardImg1 from "@/assets/img/PointCardImg1.png";
import PointCardImg2 from "@/assets/img/PointCardImg2.png";
import { ref, onMounted } from "vue";
import "./PointBuy.css";

const PointBuyCards = [
  {
    image: PointCardImg1,
    name: "Olivia",
    money: "Funded $200",
    subtitles: [
      {
        subtitle: "Premium Trading",
        rotate: "-45deg",
        top: "-10px",
        left: "-90px",
        color: "#ffb107",
        zIndex: 9,
      },
      {
        subtitle: "Revenue Sharing",
        rotate: "30deg",
        top: "-30px",
        right: "-70px",
        color: "#fe8cfd",
        zIndex: 1,
      },
      {
        subtitle: "More Benefits",
        rotate: "-30deg",
        top: "145px",
        right: "-50px",
        color: "#4cbaff",
        zIndex: 9,
      },
    ],
  },
  {
    image: PointCardImg2,
    name: "Alex",
    money: "Funded $200",
    subtitles: [
      {
        subtitle: "Premium Trading",
        rotate: "-20deg",
        top: "300px",
        right: "-60px",
        color: "#966ef3",
        zIndex: 9,
      },
      {
        subtitle: "Revenue Sharing",
        rotate: "25deg",
        top: "-30px",
        right: "-60px",
        color: "#eef36e",
        zIndex: 0,
      },
      {
        subtitle: "More Benefits",
        rotate: "20deg",
        top: "130px",
        left: "-55px",
        color: "#6ef3d8",
        zIndex: 9,
      },
    ],
  },
  {
    image: PointCardImg1,
    name: "Olivia 2",
    money: "Funded $200 2",
    subtitles: [
      {
        subtitle: "Premium Trading",
        rotate: "-30deg",
        top: "-5px",
        left: "-85px",
        color: "#a8f36e",
        zIndex: 9,
      },
      {
        subtitle: "Revenue Sharing",
        rotate: "40deg",
        top: "-35px",
        right: "-90px",
        color: "#f38d6e",
        zIndex: 9,
      },
      {
        subtitle: "More Benefits",
        rotate: "-25deg",
        top: "140px",
        right: "-65px",
        color: "#6e75f3",
        zIndex: 9,
      },
    ],
  },
  {
    image: PointCardImg2,
    name: "Alex 2",
    money: "Funded $200 2",
    subtitles: [
      {
        subtitle: "Premium Trading",
        rotate: "-50deg",
        top: "-25px",
        left: "-105px",
        color: "#f36e70",
        zIndex: 9,
      },
      {
        subtitle: "Revenue Sharing",
        rotate: "20deg",
        top: "-50px",
        right: "-75px",
        color: "#eef36e",
        zIndex: 9,
      },
      {
        subtitle: "More Benefits",
        rotate: "-20deg",
        top: "155px",
        right: "-45px",
        color: "#6ef3d8",
        zIndex: 9,
      },
    ],
  },
];

const cardsScroll = ref(null);
const activeIndex = ref(0);

onMounted(() => {
  const container = cardsScroll.value;
  let isScrolling;

  container.addEventListener("scroll", () => {
    clearTimeout(isScrolling);
    isScrolling = setTimeout(() => {
      const children = Array.from(container.children);
      const containerCenter = container.scrollLeft + container.offsetWidth / 2;

      let closestCard = null;
      let closestDistance = Infinity;
      let closestIndex = 0;

      children.forEach((card, idx) => {
        const cardCenter = card.offsetLeft + card.offsetWidth / 2;
        const distance = Math.abs(containerCenter - cardCenter);
        if (distance < closestDistance) {
          closestDistance = distance;
          closestCard = card;
          closestIndex = idx;
        }
      });

      if (closestCard) {
        activeIndex.value = closestIndex;
        container.scrollTo({
          left:
            closestCard.offsetLeft -
            container.offsetWidth / 2 +
            closestCard.offsetWidth / 2,
          behavior: "smooth",
        });
      }
    }, 100);
  });
});
</script>

<template>
  <div class="pointBuy">
    <div class="pointBuyTexts">
      <AnimatedText
        class="pointBuyTitle Title"
        text="Not for Everyone. That’s the Point."
      />

      <AnimatedText
        anim-delay="0.02"
        class="Subtitle pointBuySubtitle"
        text="FundFix is built for serious capital allocators —not hype-chasers."
      />
    </div>

    <div class="pointBuyRightWrapper">
      <div
        class="pointBuySubtitles"
        v-for="(card, index) in PointBuyCards"
        :key="index"
      >
        <div
          v-for="(sub, i) in card.subtitles"
          :key="i"
          :class="[
            'pointBuyCardSubtitle',
            activeIndex === index ? 'active' : 'inactive',
          ]"
          :style="{
            rotate: sub.rotate,
            top: sub.top,
            left: sub.left,
            right: sub.right,
            background: sub.color,
            zIndex: sub.zIndex || 'auto',
          }"
        >
          <div class="pointBuyCardSubtitleCircle" />
          {{ sub.subtitle }}
        </div>
      </div>

      <div class="pointBuyRight">
        <div class="pointBuyCardText">
          <div class="UnderTitle pointBuyUnderTitle">
            <Stars color="#0f0f0f" /> Exclusivity
          </div>
          <div class="pointBuyCardTitle">
            Join early to unlock access, bonuses, and FundFix from day one.
          </div>
        </div>

        <div class="pointBuyCards">
          <div class="pointBuyCardsScroll" ref="cardsScroll">
            <div
              class="pointBuyCard"
              v-for="(card, index) in PointBuyCards"
              :key="index"
            >
              <div class="pointBuyCardImg">
                <img :src="card.image" alt=" " />
              </div>
              <div class="pointBuyCardTexts">
                <div class="pointBuyCardName">{{ card.name }}</div>
                <div class="pointBuyCardMoney">{{ card.money }}</div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped></style>

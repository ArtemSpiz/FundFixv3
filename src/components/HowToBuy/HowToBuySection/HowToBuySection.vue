<script setup>
import Stars from "@/assets/svg/Stars.vue";
import "./HowToBuySection.css";

import HowBuyCardImage1 from "@/assets/img/HowBuyCardImage1.png";
import HowBuyCardImage2 from "@/assets/img/HowBuyCardImage2.png";
import HowBuyCardImage3 from "@/assets/img/HowBuyCardImage3.png";
import HowBuyCardImage4 from "@/assets/img/HowBuyCardImage4.png";
import AnimatedText from "@/components/AnimatedText.vue";

const HowBuyCards = [
  {
    image: HowBuyCardImage1,
    title: "Setup Wallet",
    subtitle: "Connect via Wallet Connect if needed",
    number: "01",
  },
  {
    image: HowBuyCardImage2,
    title: "Choose Currency",
    subtitle: "Supported: ETH, BNB, MATIC, USDT, BUSD, USDC",
    number: "02",
  },
  {
    image: HowBuyCardImage3,
    title: "Buy FIX",
    subtitle: "Enter the amount, confirm in your wallet, pay gas fee",
    number: "03",
  },
  {
    image: HowBuyCardImage4,
    title: "Receive Tokens",
    subtitle:
      "Tokens will be airdropped after presale. Track assets in your wallet",
    number: "04",
  },
];
</script>

<template>
  <div class="howBuy">
    <div class="howBuyTexts">
      <div class="howBuyTitles">
        <div class="UnderTitle"><Stars /> How it works</div>
        <AnimatedText class="howBuyTitle" text="How to buy" />
      </div>

      <AnimatedText
        class="howBuySubtitle Subtitle"
        anim-delay="0.01"
        text="  Easily purchase TICS in just a few steps. Connect your wallet, choose your currency, and follow the guided process — simple, secure, and fast"
      />
    </div>

    <div class="howBuyCards">
      <div
        class="howBuyCardWrapper"
        v-for="(card, index) in HowBuyCards"
        :key="index"
        :class="{
          sec: index === 1,
          third: index === 2,
          fourth: index === 3,
        }"
      >
        <div class="howBuyCard">
          <div class="howBuyCardTop">
            <div class="howBuyCardImg">
              <img :src="card.image" alt=" " />
            </div>
            <div class="howBuyCardText">
              <div class="howBuyCardTitle">{{ card.title }}</div>
              <div class="howBuyCardSubtitle">{{ card.subtitle }}</div>
            </div>
          </div>

          <div class="howBuyCardNum">{{ card.number }}</div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped></style>

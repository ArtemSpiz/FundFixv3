<script setup>
import ArrowFAQ from "@/assets/svg/ArrowFAQ.vue";
import Stars from "@/assets/svg/Stars.vue";
import "./FAQ.css";
import { ref } from "vue";

const FaqCards = [
  {
    title: "Who can invest in FundFix?",
    subtitle:
      "Yes. Smart contracts audited by CertiK, funds secured via multi-sig, full KYC/AML compliance.",
  },
  {
    title: "Is this another crypto launchpad?",
    subtitle:
      "Yes. Smart contracts audited by CertiK, funds secured via multi-sig, full KYC/AML compliance.",
  },
  {
    title: "Do I need to be accredited?",
    subtitle:
      "Yes. Smart contracts audited by CertiK, funds secured via multi-sig, full KYC/AML compliance.",
  },
  {
    title: "Are the deals legit?",
    subtitle:
      "Yes. Smart contracts audited by CertiK, funds secured via multi-sig, full KYC/AML compliance.",
  },
  {
    title: "Is this secure?",
    subtitle:
      "Yes. Smart contracts audited by CertiK, funds secured via multi-sig, full KYC/AML compliance.",
  },
];

const opened = ref(Array(FaqCards.length).fill(false));

const toggle = (index) => {
  opened.value[index] = !opened.value[index];
};
</script>

<template>
  <div class="faq">
    <div class="faqTexts">
      <div class="UnderTitle"><Stars />FAQ</div>
      <div class="Title faqTitle">Frequently Asked Questions</div>
    </div>
    <div class="faqCards">
      <div
        class="faqCard"
        v-for="(card, index) in FaqCards"
        :key="index"
        @click="toggle(index)"
      >
        <div class="faqCardTop">
          <div class="faqCardTitle">{{ card.title }}</div>
          <div class="faqCardArrow" :class="{ rotated: opened[index] }">
            <ArrowFAQ />
          </div>
        </div>

        <transition name="faq-slide">
          <div v-show="opened[index]" class="faqCardSubtitle">
            {{ card.subtitle }}
          </div>
        </transition>
      </div>
    </div>
  </div>
</template>

<style scoped></style>

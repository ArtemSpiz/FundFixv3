<script setup>
import { ref, defineProps, onMounted, onUnmounted } from "vue";
import "./Header.css";
import logo from "@/assets/img/logoHeader.png";
import Buger from "@/assets/svg/Buger.vue";

const HeaderLinks = [
  { title: "Home", target: "#home" },
  { title: "Product", target: "#product" },
  { title: "About", target: "#about" },
  { title: "How it works", target: "#how-it-works" },
  { title: "Tokenomics", target: "#tokenomics" },
];

const isScaledIn = ref(false);

const activeLink = ref(0);

function setActive(index) {
  activeLink.value = index;
}

const props = defineProps({
  onScrollToTokenomics: Function,
  isOnRoadmap: Boolean,
  isOnTokenomics: Boolean,
});

function scrollToSection(target, index) {
  if (target === "#tokenomics") {
    props.onScrollToTokenomics?.();
    activeLink.value = index;
    return;
  }

  const el = document.querySelector(target);
  if (el) {
    const top = el.getBoundingClientRect().top + window.pageYOffset;
    window.scrollTo({
      top,
      behavior: "smooth",
    });
    activeLink.value = index;
  }
}

const isOnWhite = ref(false);
const whiteSection = ref(null);
let observer = null;

onMounted(() => {
  observer = new IntersectionObserver(
    ([entry]) => {
      isOnWhite.value = entry.isIntersecting;
    },
    { threshold: 0.1 }
  );

  if (whiteSection.value) {
    observer.observe(whiteSection.value);
  }

  setTimeout(() => {
    isScaledIn.value = true;
  }, 100);
});

onUnmounted(() => {
  if (observer && whiteSection.value) {
    observer.unobserve(whiteSection.value);
  }
});
</script>

<template>
  <div class="headerWrapper">
    <div class="header single-pulse-wave" :class="{ 'scaled-in': isScaledIn }">
      <div class="headerLogo"><img :src="logo" alt="" /></div>
      <div class="headerRight">
        <div class="headerLinks">
          <div v-for="(link, index) in HeaderLinks" :key="index">
            <div
              class="headerLink"
              @click="scrollToSection(link.target, index)"
              @mouseenter="setActive(index)"
              @mouseleave="setActive(0)"
              :class="{
                active: index === activeLink,
                'on-roadmap': props.isOnRoadmap || props.isOnTokenomics,
              }"
            >
              {{ link.title }}
            </div>
          </div>
        </div>

        <button
          :class="{
            'on-roadmap': props.isOnRoadmap || props.isOnTokenomics,
          }"
          class="headerBtn jelly-wave"
        >
          Connect Wallet
        </button>

        <Buger class="burgerOpen" />
      </div>
    </div>
  </div>
</template>

<style scoped></style>

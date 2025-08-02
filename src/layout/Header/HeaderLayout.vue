<script setup>
import { ref } from "vue";
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

const activeLink = ref(0);

function setActive(index) {
  activeLink.value = index;
}

function scrollToSection(target, index) {
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
</script>

<template>
  <div class="header single-pulse-wave">
    <div class="headerLogo"><img :src="logo" alt="" /></div>
    <div class="headerRight">
      <div class="headerLinks">
        <div v-for="(link, index) in HeaderLinks" :key="index">
          <div
            class="headerLink"
            @click="scrollToSection(link.target, index)"
            @mouseenter="setActive(index)"
            @mouseleave="setActive(0)"
            :class="{ active: index === activeLink }"
          >
            {{ link.title }}
          </div>
        </div>
      </div>

      <button class="headerBtn jelly-wave">Connect Wallet</button>

      <Buger class="burgerOpen"/>
    </div>
  </div>
</template>

<style scoped></style>

<script setup>
import { ref } from "vue";
import "./Header.css";
import logo from "@/assets/img/logoHeader.png";

const HeaderLinks = [
  {
    title: "Home",
  },
  {
    title: "Product",
  },
  {
    title: "About",
  },
  {
    title: "How it works",
  },
  {
    title: "Tokenomics",
  },
];

const activeLink = ref(0);

function setActive(index) {
  activeLink.value = index;
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
            @click="setActive(index)"
            :class="{ active: index === activeLink }"
          >
            {{ link.title }}
          </div>
        </div>
      </div>

      <button class="headerBtn jelly-wave">Connect Wallet</button>
    </div>
  </div>
</template>

<style lang="scss" scoped></style>

<script setup>
import logo from "@/assets/img/logoFooter.png";
import Discord from "@/assets/svg/discord.vue";
import Linkedin from "@/assets/svg/linkedin.vue";
import X from "@/assets/svg/x.vue";

import "./Footer.css";
import FooterSocBg from "@/assets/svg/footerSocBg.vue";

const FooterLinks = [
  { title: "Home" },
  { title: "Accessibility" },
  { title: "Utility Token" },
  { title: "Next-Gen Investing" },
  { title: "Tokenomics" },
  { title: "Roadmap" },
  { title: "FAQ" },
];

const FooterSoc = [
  {
    icon: Linkedin,
  },
  {
    icon: Discord,
  },
  {
    icon: X,
  },
];
</script>

<template>
  <div class="footer">
    <div class="footerLogo">
      <img :src="logo" alt="" />
    </div>

    <div class="footerRight">
      <div class="footerLinks">
        <div
          class="footerLink"
          v-for="(link, index) in FooterLinks"
          :key="index"
        >
          {{ link.title }}
        </div>
      </div>

      <div class="footerSocWrap">
        <div
          class="footerSoc"
          v-for="(social, index) in FooterSoc"
          :key="index"
        >
          <FooterSocBg />
          <component :is="social.icon" class="footerSocIcon"/>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped></style>

<script setup>
import Stars from "@/assets/svg/Stars.vue";
import "./Accessibility.css";
import { onMounted, onUnmounted } from "vue";
import gsap from "gsap";
import ScrollTrigger from "gsap/ScrollTrigger";

gsap.registerPlugin(ScrollTrigger);

const AccessibilityCard = [
  {
    title: "The Power of Tokenization",
    text: "Tokenized models offer better liquidity and broader investor access.",
    subtitles: [
      { subtitle: "Tokenization" },
      { subtitle: "Fintech" },
      { subtitle: "Liquidity" },
    ],
  },
  {
    title: "How FundFix Works",
    text: "With $FIX and accreditation, anyone can access top private deals easily.",
    subtitles: [
      { subtitle: "Mission" },
      { subtitle: "Accessibility" },
      { subtitle: "Blockchain" },
    ],
  },
];

let scrollTimeout = null;
let isScrolling = false;

onMounted(() => {
  const section = document.querySelector(".accessibility");
  const cards = gsap.utils.toArray(".accessibilityCard");
  const texts = document.querySelector(".accessibilityTexts");

  cards.forEach((card, i) => {
    gsap.set(card, {
      yPercent: i * 20,
      opacity: 0,
      scaleY: 1,
      transformOrigin: "center center",
    });
  });

  gsap.set(texts, {
    yPercent: 0,
    opacity: 1,
  });

  const masterTimeline = gsap.timeline({
    scrollTrigger: {
      id: "accessAnimation",
      trigger: section,
      start: "top top",
      end: () => `+=${cards.length * 600 + 600}`,
      scrub: 1,
      pin: true,
      onUpdate: () => {
        isScrolling = true;

        if (scrollTimeout) {
          clearTimeout(scrollTimeout);
        }

        scrollTimeout = setTimeout(() => {
          isScrolling = false;
          cards.forEach((card) => {
            gsap.to(card, {
              scaleY: 1,
              duration: 0.6,
              ease: "power2.out",
            });
          });
        }, 100);
      },
    },
  });

  masterTimeline.to(cards, {
    yPercent: "-=450",
    ease: "none",
    duration: 0.7,
  });

  masterTimeline.to(
    texts,
    {
      yPercent: 100,
      ease: "power2.inOut",
      duration: 0.5,
    },
    "-=0.4"
  );

  cards.forEach((card) => {
    gsap
      .timeline({
        scrollTrigger: {
          trigger: card,
          start: "top bottom-=50",
          end: "bottom top+=100",
          scrub: true,
          onUpdate: (self) => {
            if (isScrolling) {
              const progress = self.progress;
              let scaleValue = 1;

              if (progress < 0.5) {
                scaleValue = 1 + progress * 0.4;
              } else {
                scaleValue = 1.2 - (progress - 0.5) * 0.4;
              }

              gsap.set(card, { scaleY: scaleValue });
            }
          },
        },
      })
      .to(card, {
        opacity: 1,
        ease: "power1.out",
        duration: 1,
      })
      .to(
        card,
        {
          opacity: 0,
          ease: "power1.in",
          duration: 1,
        },
        ">"
      );
  });

  let lastScrollY = window.scrollY;
  let scrollVelocity = 0;

  const handleScroll = () => {
    const currentScrollY = window.scrollY;
    scrollVelocity = Math.abs(currentScrollY - lastScrollY);
    lastScrollY = currentScrollY;

    if (scrollVelocity > 1.2) {
      cards.forEach((card) => {
        const rect = card.getBoundingClientRect();
        const isVisible = rect.top < window.innerHeight && rect.bottom > 0;

        if (isVisible) {
          gsap.to(card, {
            scaleY: 1.2,
            duration: 0.6,
            ease: "power2.out",
          });
        }
      });
    }
  };

  window.addEventListener("scroll", handleScroll, { passive: true });
  section._scrollHandler = handleScroll;
});

function killAnimations() {
  const mainTrigger = ScrollTrigger.getById("accessAnimation");
  if (mainTrigger) mainTrigger.kill();

  ScrollTrigger.getAll().forEach((trigger) => {
    if (trigger.trigger && trigger.trigger.closest(".accessibility")) {
      trigger.kill();
    }
  });

  if (scrollTimeout) {
    clearTimeout(scrollTimeout);
  }

  const section = document.querySelector(".accessibility");
  if (section && section._scrollHandler) {
    window.removeEventListener("scroll", section._scrollHandler);
  }
}

onUnmounted(() => {
  killAnimations();
});
</script>

<template>
  <div class="accessibility">
    <div class="accessibilityTexts">
      <div class="UnderTitle"><Stars />Accessibility</div>

      <div class="accessibilityTitle">
        FundFix: Private Markets for Everyone
      </div>
    </div>

    <div class="accessibilityCards">
      <div
        class="accessibilityCard"
        v-for="(card, index) in AccessibilityCard"
        :key="index"
        :class="{ secondCard: index === 1 }"
      >
        <div class="accessCardSubtitles">
          <div
            class="accessCardSubtitle"
            v-for="(subtitle, i) in card.subtitles"
            :key="i"
            :class="{
              firstSub: index === 0 && i === 0,
              secSub: index === 0 && i === 1,
              thirdSub: index === 0 && i === 2,
              firstSubSecCard: index === 1 && i === 0,
              secSubSecCard: index === 1 && i === 1,
              thirdSubSecCard: index === 1 && i === 2,
            }"
          >
            {{ subtitle.subtitle }}
          </div>
        </div>

        <div class="accessCardTexts">
          <div class="accessCardTitle">{{ card.title }}</div>
          <div class="accessCardText">{{ card.text }}</div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped></style>

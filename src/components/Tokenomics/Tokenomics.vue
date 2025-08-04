<script setup>
import { onMounted, onUnmounted, inject, watch } from "vue";
import Stars from "@/assets/svg/Stars.vue";
import "./Tokenomics.css";
import TokenomicsPercents from "./TokenomicsPercents/TokenomicsPercents.vue";
import AnimatedText from "../AnimatedText.vue";
import TokenomicsIcon1 from "@/assets/svg/TokenomicsIcon1.vue";
import "@/components/Tokenomics/TokenomicsCards/TokenomicsCards.css";
import TokenomicsIcon2 from "@/assets/svg/TokenomicsIcon2.vue";
import TokenomicsIcon3 from "@/assets/svg/TokenomicsIcon3.vue";
import TokenomicsIcon4 from "@/assets/svg/TokenomicsIcon4.vue";
import TokenomicsIcon5 from "@/assets/svg/TokenomicsIcon5.vue";
import BgTokCards from "@/assets/svg/bgTokCards.vue";
import Arrow from "@/assets/svg/Arrow.vue";
import gsap from "gsap";
import { ScrollTrigger } from "gsap/ScrollTrigger";

gsap.registerPlugin(ScrollTrigger);

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

const isMobile = window.innerWidth < 769;
const isMiniMobile = window.innerWidth < 641;

// Отримуємо сигнал від Future компонента
const shouldStartTokenomicsAnimation = inject(
  "shouldStartTokenomicsAnimation",
  null
);

let manualPositions = [
  { x: -350, y: 100 },
  { x: 150, y: 250 },
  { x: -250, y: 400 },
  { x: 150, y: 150 },
  { x: 250, y: 300 },
];

if (isMiniMobile) {
  manualPositions = [
    { x: -160, y: 100 },
    { x: -160, y: 250 },
    { x: -160, y: 400 },
    { x: 160, y: 150 },
    { x: 160, y: 300 },
  ];
}

// Функції для блокування/розблокування скролу
function disableScroll() {
  document.body.style.overflow = "hidden";
  document.documentElement.style.overflow = "hidden";
}

function enableScroll() {
  document.body.style.overflow = "";
  document.documentElement.style.overflow = "";
}

// Функція для ініціалізації IntersectionObserver (для звичайної токеноміки)
function initIntersectionObserver() {
  const observer = new IntersectionObserver(
    (entries) => {
      entries.forEach((entry) => {
        if (entry.isIntersecting) {
          entry.target.classList.add("show");
        } else {
          entry.target.classList.remove("show");
        }
      });
    },
    { threshold: isMiniMobile ? 0.5 : isMobile ? 0.8 : 0.7 }
  );

  document.querySelectorAll(".animate-on-scroll").forEach((el) => {
    el.classList.add("hidden");
    observer.observe(el);
  });
}

// Функція для запуску анімації карток (звичайна токеноміка)
function initNormalTokenomicsAnimation() {
  const cards = document.querySelectorAll(".tokenomicsCardWrapper");

  if (cards.length === 0) return;

  cards.forEach((card, i) => {
    const pos = manualPositions[i] || { x: 0, y: 0 };
    gsap.set(card, {
      x: pos.x,
      y: window.innerHeight + pos.y,
      opacity: 1,
      position: "absolute",
    });
  });

  const tl = gsap.timeline({
    scrollTrigger: {
      trigger: ".tokenomics",
      start: "top top",
      end: () => `+=${cards.length * 600}`,
      scrub: true,
      pin: true,
      id: "tokenomicsCardsAnim",
      anticipatePin: 1,
    },
  });

  cards.forEach((card, i) => {
    const targetY = manualPositions[i]?.y || 0;

    tl.to(card, {
      opacity: 1,
      y: targetY,
      duration: 2,
      ease: "power3.inOut",
    });
  });

  // Пауза на пік
  tl.to({}, { duration: 1 });

  // Потім рух вгору
  cards.forEach((card, i) => {
    const targetY = manualPositions[i]?.y || 0;

    tl.to(
      card,
      {
        opacity: 1,
        y: targetY,
        duration: 1.4, // трохи швидше
        ease: "power3.out",
      },
      i * 0.08 // майже одночасно (було 0.3)
    );
  });
}

// Функція для запуску анімації карток всередині Future
function initFutureTokenomicsAnimation() {
  console.log("initFutureTokenomicsAnimation called");

  const cards = document.querySelectorAll(".tokenomicsCardWrapper");
  console.log("Found cards:", cards.length);

  if (cards.length === 0) {
    // Якщо картки ще не знайдені, спробуємо ще раз через невеликий час
    setTimeout(() => {
      const retryCards = document.querySelectorAll(".tokenomicsCardWrapper");
      console.log("Retry - Found cards:", retryCards.length);
      if (retryCards.length > 0) {
        startCardsAnimation(retryCards);
      }
    }, 100);
    return;
  }

  startCardsAnimation(cards);
}

function startCardsAnimation(cards) {
  console.log("Starting cards animation with", cards.length, "cards");

  // Блокуємо скрол на початку анімації
  disableScroll();

  // Спочатку показуємо текстові елементи
  document.querySelectorAll(".animate-on-scroll").forEach((el) => {
    el.classList.add("show");
  });

  // Встановлюємо початкові позиції для карток
  cards.forEach((card, i) => {
    const pos = manualPositions[i] || { x: 0, y: 0 };
    gsap.set(card, {
      x: pos.x,
      y: window.innerHeight + pos.y,
      opacity: 1,
      position: "absolute",
      scale: 1,
    });
    console.log(`Card ${i} set to position:`, pos);
  });

  // Створюємо timeline для анімації карток
  const tl = gsap.timeline({
    onComplete: () => {
      // Розблокуємо скрол після завершення анімації
      enableScroll();
      console.log("Animation completed, scroll enabled");
    },
  });

  // Анімуємо картки одну за одною до їх цільових позицій
  cards.forEach((card, i) => {
    tl.to(
      card,
      {
        y: manualPositions[i]?.y || 0,
        duration: 1.2,
        ease: "power2.out",
      },
      i * 0.1 // Збільшена затримка між картками для плавності
    );
  });

  // Додаємо паузу перед рухом вгору

  // Потім анімуємо картки вгору одну за одною
  cards.forEach((card, i) => {
    tl.to(
      card,
      {
        y: -window.innerHeight - 600,
        opacity: 1,
        duration: 1.6,
        ease: "power2.inOut",
      },
      `>-${1 - i * 0.15}` // ближче одне до одного
    );
  });

  console.log("Timeline created and started");
}

// Слідкуємо за сигналом від Future компонента
if (shouldStartTokenomicsAnimation) {
  console.log("Watching for shouldStartTokenomicsAnimation");

  watch(
    shouldStartTokenomicsAnimation,
    (newValue) => {
      console.log("shouldStartTokenomicsAnimation changed to:", newValue);
      if (newValue) {
        console.log(
          "Future animation completed, starting tokenomics animation"
        );
        // Невелика затримка для забезпечення готовності DOM
        setTimeout(() => {
          initFutureTokenomicsAnimation();
        }, 300);
      }
    },
    { immediate: true }
  );
} else {
  console.log(
    "No shouldStartTokenomicsAnimation signal, using normal animation"
  );
  // Якщо токеноміка не всередині Future, запускаємо звичайну анімацію
  onMounted(() => {
    initIntersectionObserver();
    initNormalTokenomicsAnimation();
  });
}

onUnmounted(() => {
  // Розблокуємо скрол при виході з компонента
  enableScroll();
  ScrollTrigger.getById("tokenomicsCardsAnim")?.kill();
});
</script>

<template>
  <div ref="whiteSection" id="tokenomics" class="tokenomics">
    <div class="tokenomicsTexts animate-on-scroll">
      <div class="UnderTitle animate-on-scroll">
        <Stars />
        <AnimatedText text="Transparent Structure" />
      </div>
      <AnimatedText class="Title tokenomicsTitle" text="Our Tokenomics" />
    </div>

    <div class="animate-on-scroll">
      <TokenomicsPercents class="TokenomicsPercentsWrap" />
    </div>

    <div class="wrapperCardsToc">
      <div class="tokenomicsCardsWrapper">
        <div class="tokenomicsCards">
          <div class="tokenomicsCardsScroll">
            <div
              class="tokenomicsCardWrapper"
              v-for="(card, index) in TokenomicsCards"
              :key="index"
              :class="{
                centeredCards: index === 3,
                centeredCardsSec: index === 4,
              }"
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
          <div class="tokenomicsCardsArrow left" @click="scrollPrev">
            <Arrow />
          </div>
          <div class="tokenomicsCardsArrow" @click="scrollNext">
            <Arrow />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped></style>

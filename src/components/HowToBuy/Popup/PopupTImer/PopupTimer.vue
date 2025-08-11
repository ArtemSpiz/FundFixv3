<script setup>
import { ref, onMounted, onUnmounted, computed, nextTick } from "vue";
import "./PopupTimer.css";
import ClockLine from "@/assets/svg/ClockLine.vue";

const hours = ref("12");
const minutes = ref("12");
const seconds = ref("32");

const nextHours = ref("12");
const nextMinutes = ref("12");
const nextSeconds = ref("32");

const hoursFlip = ref(false);
const minutesFlip = ref(false);
const secondsFlip = ref(false);

// Добавляем реактивное значение для прогресса
const progress = ref(52); // 52% начальное значение
const rangeInput = ref(null); // ссылка на input элемент

// Вычисляемое свойство для отображения процентов
const progressPercent = computed(() => {
  return (progress.value / 100).toFixed(2) + "%";
});

// Функция для обновления стиля input range
function updateRangeBackground() {
  if (rangeInput.value) {
    const value = progress.value;
    const background = `linear-gradient(
      to right,
      rgba(238, 193, 190, 0.92) 0%,
      rgba(148, 160, 232, 0.92) ${value}%,
      transparent ${value}%,
      transparent 100%
    )`;
    rangeInput.value.style.background = background;
  }
}

// Обработчик изменения значения input
function handleRangeChange() {
  updateRangeBackground();
}

let timerInterval;

function triggerFlip(unit) {
  if (unit === "hours") hoursFlip.value = true;
  if (unit === "minutes") minutesFlip.value = true;
  if (unit === "seconds") secondsFlip.value = true;

  setTimeout(() => {
    if (unit === "hours") hoursFlip.value = false;
    if (unit === "minutes") minutesFlip.value = false;
    if (unit === "seconds") secondsFlip.value = false;
  }, 800);
}

function startTimer() {
  let totalSeconds =
    parseInt(hours.value) * 3600 +
    parseInt(minutes.value) * 60 +
    parseInt(seconds.value);

  timerInterval = setInterval(() => {
    if (totalSeconds <= 0) {
      clearInterval(timerInterval);
      return;
    }

    const prevHours = hours.value;
    const prevMinutes = minutes.value;
    const prevSeconds = seconds.value;

    totalSeconds--;

    const h = Math.floor(totalSeconds / 3600);
    const m = Math.floor((totalSeconds % 3600) / 60);
    const s = totalSeconds % 60;

    const newHours = String(h).padStart(2, "0");
    const newMinutes = String(m).padStart(2, "0");
    const newSeconds = String(s).padStart(2, "0");

    if (newSeconds !== prevSeconds) {
      nextSeconds.value = newSeconds;
      triggerFlip("seconds");
    }
    if (newMinutes !== prevMinutes) {
      nextMinutes.value = newMinutes;
      triggerFlip("minutes");
    }
    if (newHours !== prevHours) {
      nextHours.value = newHours;
      triggerFlip("hours");
    }

    setTimeout(() => {
      hours.value = newHours;
      minutes.value = newMinutes;
      seconds.value = newSeconds;
    }, 250);
  }, 1000);
}

onMounted(() => {
  startTimer();
  // Устанавливаем начальный стиль после монтирования
  nextTick(() => {
    updateRangeBackground();
  });
});

onUnmounted(() => {
  clearInterval(timerInterval);
});
</script>

<template>
  <div class="popupTop">
    <div class="popupClock">
      <div class="popupClockTitle">BUY $FIX</div>

      <div class="flip-clock">
        <div class="flipUnitWrapper">
          <div class="flip-unit" :class="{ flipping: hoursFlip }">
            <div class="flip-unit-static-top">
              <div class="flip-digit">{{ hours }}</div>
            </div>
            <div class="flip-unit-static-bottom">
              <div class="flip-digit">{{ nextHours }}</div>
            </div>
            <div class="flip-unit-flip-top">
              <div class="flip-digit">{{ hours }}</div>
            </div>
            <div class="flip-unit-flip-bottom">
              <div class="flip-digit">{{ nextHours }}</div>
            </div>
            <ClockLine class="flip-unit-line" />
            <div class="flip-unit-shadow"></div>
          </div>

          <div class="flip-clock-title">Days</div>
        </div>
        <div class="flipUnitWrapper">
          <div class="flip-unit" :class="{ flipping: minutesFlip }">
            <div class="flip-unit-static-top">
              <div class="flip-digit">{{ minutes }}</div>
            </div>
            <div class="flip-unit-static-bottom">
              <div class="flip-digit">{{ nextMinutes }}</div>
            </div>
            <div class="flip-unit-flip-top">
              <div class="flip-digit">{{ minutes }}</div>
            </div>
            <div class="flip-unit-flip-bottom">
              <div class="flip-digit">{{ nextMinutes }}</div>
            </div>
            <ClockLine class="flip-unit-line" />
            <div class="flip-unit-shadow"></div>
          </div>
          <div class="flip-clock-title">Hours</div>
        </div>
        <div class="flipUnitWrapper">
          <div class="flip-unit" :class="{ flipping: secondsFlip }">
            <div class="flip-unit-static-top">
              <div class="flip-digit">{{ seconds }}</div>
            </div>
            <div class="flip-unit-static-bottom">
              <div class="flip-digit">{{ nextSeconds }}</div>
            </div>
            <div class="flip-unit-flip-top">
              <div class="flip-digit">{{ seconds }}</div>
            </div>
            <div class="flip-unit-flip-bottom">
              <div class="flip-digit">{{ nextSeconds }}</div>
            </div>
            <ClockLine class="flip-unit-line" />
            <div class="flip-unit-shadow"></div>
          </div>
          <div class="flip-clock-title">Mins</div>
        </div>
      </div>
    </div>

    <div class="popupTopContent">
      <div class="popupTopTexts">
        <div class="popupTopTitles">
          <div class="popupTopTitle">USD Raised</div>
          <div class="popupTopSubtitle">$18,434,064</div>
        </div>
        <div class="popupTopSubtitle">{{ progressPercent }}</div>
      </div>
      <div class="popupTopInputRangeWrapper">
        <input
          ref="rangeInput"
          class="popupInputRange"
          min="0"
          max="100"
          v-model="progress"
          type="range"
          @input="handleRangeChange"
          @change="handleRangeChange"
        />
      </div>
    </div>
  </div>
</template>

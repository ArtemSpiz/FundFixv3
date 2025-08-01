<script setup>
import { onMounted, ref, defineProps } from "vue";

const props = defineProps({
  text: {
    type: String,
    required: true,
  },
  threshold: {
    type: Number,
    default: 0.5,
  },
  once: {
    type: Boolean,
    default: true,
  },
});

const letters = props.text.split("");
const isVisible = ref(false);
const animatedRef = ref(null);

onMounted(() => {
  const observer = new IntersectionObserver(
    ([entry]) => {
      if (entry.isIntersecting) {
        isVisible.value = true;
        if (props.once) observer.disconnect();
      } else if (!props.once) {
        isVisible.value = false;
      }
    },
    { threshold: props.threshold }
  );

  if (animatedRef.value) {
    observer.observe(animatedRef.value);
  }
});
</script>

<template>
  <div class="animated-text" ref="animatedRef">
    <span
      v-for="(letter, index) in letters"
      :key="index"
      :class="['letter', { visible: isVisible }]"
      :style="isVisible ? { animationDelay: `${index * 0.03}s` } : {}"
    >
      {{ letter === " " ? "\u00A0" : letter }}
    </span>
  </div>
</template>

<style scoped>
.animated-text {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5px;
  overflow: hidden;
  justify-content: center;
  align-items: center;
}

.letter {
  display: inline-flex;
  opacity: 0;
  transform: translateY(20px);
}

.letter.visible {
  animation: riseFadeIn 0.4s forwards ease-out;
}

@keyframes riseFadeIn {
  to {
    opacity: 1;
    transform: translateY(0);
  }
}
</style>

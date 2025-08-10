<script setup>
import { computed, ref } from "vue";
import ActiveCircle from "@/assets/svg/ActiveCircle.vue";
import DropArrow from "@/assets/svg/DropArrow.vue";
import IconConverBtc from "@/assets/svg/IconConverBtc.vue";
import logo from "@/assets/img/logoHeader.png";
import IconConvert from "@/assets/svg/IconConvert.vue";

const ConvertValues = [
  {
    icon: IconConverBtc,
    title: "BTC",
  },
  {
    icon: IconConvert,
    title: "USD",
  },
];

const isOpen = ref(false);

function openDropList() {
  isOpen.value = !isOpen.value;
}

const selected = ref("card");

function selectRadio(value) {
  selected.value = value;
}

const inputAmount = ref(1);

const conversionRate = 480.870879;

const convertedAmount = computed(() => {
  const num = parseFloat(inputAmount.value);
  if (isNaN(num) || num < 0) return 0;
  return (num * conversionRate).toFixed(6);
});
</script>

<template>
  <div class="convertCenter">
    <div class="convertCenterTop">
      <div class="convertCenterTitle">Header</div>
      <div class="convertCenterTopBtns">
        <div class="convertRadioBtns">
          <div
            class="convertRadioBtn"
            @click="selectRadio('card')"
            :class="{ active: selected === 'card' }"
          >
            <div class="convertRadioBtnLeft">
              <ActiveCircle
                :fill="
                  selected === 'card'
                    ? 'url(#paint0_linear_1056_4529)'
                    : '#272727'
                "
              />
              <div class="convertRadioBtnTitle">via card</div>
            </div>

            <div class="convertRadioBtnSubtitle">(Mastercard, Visa, etc.)</div>
          </div>
          <div
            class="convertRadioBtn"
            @click="selectRadio('crypto')"
            :class="{ active: selected === 'crypto' }"
          >
            <div class="convertRadioBtnLeft">
              <ActiveCircle
                :fill="
                  selected === 'crypto'
                    ? 'url(#paint0_linear_1056_4529)'
                    : '#272727'
                "
              />
              <div class="convertRadioBtnTitle">via crypto</div>
            </div>
          </div>
        </div>

        <div
          class="convertDropBtn"
          :class="{ active: isOpen }"
          @click="openDropList()"
        >
          <div class="convertDropBtnActive">
            <div class="convertDropBtnTitle">
              <IconConverBtc />
              <span>BTC</span>
            </div>

            <DropArrow class="dropArrow" :class="{ active: isOpen }" />
          </div>

          <Transition name="dropdown">
            <div class="convertDropList" v-if="isOpen">
              <div
                class="convertDropListItem"
                v-for="(card, index) in ConvertValues"
                :key="index"
                :style="{ '--item-index': index }"
              >
                <component :is="card.icon" />
                <div class="convertDropListItemTitle">{{ card.title }}</div>
              </div>
            </div>
          </Transition>
        </div>
      </div>
    </div>

    <div class="convertCenterBot">
      <div class="convertCenterTitle">Header</div>
      <div class="convertCenterBotBtns">
        <div class="convertCenterBotBtn">
          <input class="convertCenterBotBtnTitle" v-model="inputAmount" />
          <div class="convertCenterBotBtnSubtitle">
            <IconConverBtc />
            <span>BTC</span>
          </div>
        </div>

        <div class="convertCenterBotEqual">=</div>

        <div class="convertCenterBotBtn">
          <div class="convertCenterBotBtnTitle">{{ convertedAmount }}</div>
          <div class="convertCenterBotBtnSubtitle">
            <div class="convertCenterBotBtnLogo">
              <img :src="logo" alt="" />
            </div>
            <span>FIX</span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

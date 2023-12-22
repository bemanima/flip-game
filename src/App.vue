<template>
  <div class="game">
    <GameInfo :time="timerShow" :count="countNum" />
    <GameBoard
      :time="time"
      :count="countNum"
      :isOver="isOver"
      @startTimer="startTimer"
      @stopTimer="stopTimer"
      @decreaseCount="decreaseCountNum"
      @resetTimeAndCount="resetTimeAndCount"
      @isOverHandler="isOver = $event"
    />
  </div>
</template>

<script setup>
import { ref, computed } from "vue";
import GameInfo from "./components/GameInfo.vue";
import GameBoard from "./components/GameBoard.vue";

const time = ref(120);
const countNum = ref(40);
const intervalId = ref(null);
const isOver = ref(false);

const timerShow = computed(() => {
  const minutes = Math.floor(time.value / 60)
    .toString()
    .padStart(2, "0");
  const seconds = (time.value % 60).toString().padStart(2, "0");

  return `${minutes}:${seconds}`;
});

const startTimer = () => {
  intervalId.value = setInterval(() => {
    time.value--;
    if (time.value === 0) {
      isOver.value = true;
      stopTimer();
    }
  }, 1000);
};

const stopTimer = () => {
  clearInterval(intervalId.value);
};

const decreaseCountNum = () => {
  countNum.value--;
  if (countNum.value === 0) isOver.value = true;
};

const resetTimeAndCount = () => {
  stopTimer();
  time.value = 120;
  countNum.value = 40;
};
</script>

<style scoped></style>

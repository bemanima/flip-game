<template>
  <div class="game">
    <GameInfo :time="time" :count="countNum" />
    <GameBoard />
  </div>
</template>

<script setup>
import { provide, ref } from "vue";
import GameInfo from "./components/GameInfo.vue";
import GameBoard from "./components/GameBoard.vue";

const time = ref("");
const countNum = ref(40);

const firstCard = ref(null);
const secondCard = ref(null);

const changeFirstCard = (num) => {
  firstCard.value = num;
};
const changeSecondCard = (num) => {
  secondCard.value = num;
};

provide("cardNumbers", {
  firstCard,
  secondCard,
});

provide("cardNumHandler", {
  changeFirstCard,
  changeSecondCard,
});
</script>

<style scoped></style>

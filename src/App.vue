<template>
  <div class="game">
    <GameInfo :time="time" :count="countNum" />
    <div class="game-board">
      <template v-for="(card, index) in cardArray" :key="index">
        <GameCard :cardNum="index + 1" :card="card" @click="flipCard(card)" />
      </template>
    </div>
    <GameReset v-if="finishMsg" @reset="resetGame" :msg="finishMsg" />
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";
import GameInfo from "./components/GameInfo.vue";
import GameCard from "./components/GameCard.vue";
import GameReset from "./components/GameReset.vue";
import shuffleArray from "./utils/ShuffleArray";

const cardArray = ref([]);
const time = ref(120);
const countNum = ref(40);
const firstCard = ref(null);
const secondCard = ref(null);
const intervalId = ref(null);
const finishMsg = ref("");

onMounted(() => {
  generateCardArray();
});

const generateCardArray = () => {
  for (let i = 2; i < 18; i++) {
    cardArray.value.push({
      num: i - 1,
      imgNum: Math.floor(i / 2),
      isFlipped: false,
    });
  }
  shuffleArray(cardArray.value);
};

const flipCard = (card) => {
  if (time.value === 0 || countNum.value === 0) return;
  const selectedCard = cardArray.value.find((item) => item.num === card.num);

  if (time.value === 120 && countNum.value === 40) startTimer();

  if (!selectedCard.isFlipped && !secondCard.value) {
    decreaseCountNum();

    selectedCard.isFlipped = true;

    if (!firstCard.value) {
      firstCard.value = card;
    } else {
      secondCard.value = card;
    }

    if (secondCard.value) {
      if (firstCard.value.imgNum !== secondCard.value.imgNum) {
        let timeoutId = setTimeout(() => {
          cancelFlipCard();
          clearTimeout(timeoutId);
        }, 1000);
      } else {
        resetCardsState();
      }
    }
    checkWin();
  }
};

const checkWin = () => {
  const winCondition = cardArray.value.every((card) => card.isFlipped === true);
  if (winCondition) {
    finishMsg.value = "!تبریک، شما برنده شدید";
    stopTimer();
  }
};

const cancelFlipCard = () => {
  cardArray.value.find(
    (item) => item.num === firstCard.value.num
  ).isFlipped = false;
  cardArray.value.find(
    (item) => item.num === secondCard.value.num
  ).isFlipped = false;
  resetCardsState();
};

const startTimer = () => {
  intervalId.value = setInterval(() => {
    time.value--;
    if (time.value === 0) {
      finishMsg.value = "!متاسفم، شما باختید";
      stopTimer();
    }
  }, 1000);
};

const stopTimer = () => {
  clearInterval(intervalId.value);
};

const decreaseCountNum = () => {
  countNum.value--;
  if (countNum.value === 0) {
    finishMsg.value = "!متاسفم، شما باختید";
    stopTimer();
  }
};

const resetGame = () => {
  cardArray.value = [];
  resetCardsState();
  generateCardArray();
  stopTimer();
  time.value = 120;
  countNum.value = 40;
  finishMsg.value = "";
};

const resetCardsState = () => {
  firstCard.value = null;
  secondCard.value = null;
};
</script>

<style scoped>
.game-board {
  width: 368px;
  display: flex;
  flex-wrap: wrap;
  gap: 16px;
  padding-top: 32px;
}
</style>

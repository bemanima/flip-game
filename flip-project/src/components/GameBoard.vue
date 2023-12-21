<template>
  <div class="game-board">
    <template v-for="(card, index) in randomNumbers" :key="card">
      <GameCard
        :cardNum="index + 1"
        :cardImg="card.imgNum"
        @click="flipCard(card.imgNum, index)"
      />
    </template>
  </div>
</template>

<script setup>
import { ref, onMounted, inject } from "vue";
import GameCard from "./GameCard.vue";

const { firstCard, secondCard } = inject("cardNumbers");
const { changeFirstCard, changeSecondCard } = inject("cardNumHandler");

const flipCard = (num, index) => {
  randomNumbers.value[index].isFlipped = true;
  if (!firstCard.value) {
    changeFirstCard(num);
  } else {
    changeSecondCard(num);
  }

  console.log(firstCard.value, secondCard.value);
  if (firstCard.value !== secondCard.value) {
    setTimeout(() => {
      randomNumbers.value[firstCard.value].isFlipped = false;
      randomNumbers.value[secondCard.value].isFlipped = false;
      changeFirstCard(null);
      changeSecondCard(null);
    }, 1000);
  }
};

const randomNumbers = ref([
  {
    number: 1,
    imgNum: 1,
    isFlipped: false,
  },
  {
    number: 2,
    imgNum: 2,
    isFlipped: false,
  },
  {
    number: 3,
    imgNum: 3,
    isFlipped: false,
  },
  {
    number: 4,
    imgNum: 4,
    isFlipped: false,
  },
  {
    number: 5,
    imgNum: 5,
    isFlipped: false,
  },
  {
    number: 6,
    imgNum: 6,
    isFlipped: false,
  },
  {
    number: 7,
    imgNum: 7,
    isFlipped: false,
  },
  {
    number: 8,
    imgNum: 8,
    isFlipped: false,
  },
  {
    number: 9,
    imgNum: 1,
    isFlipped: false,
  },
  {
    number: 10,
    imgNum: 2,
    isFlipped: false,
  },
  {
    number: 11,
    imgNum: 3,
    isFlipped: false,
  },
  {
    number: 12,
    imgNum: 4,
    isFlipped: false,
  },
  {
    number: 13,
    imgNum: 5,
    isFlipped: false,
  },
  {
    number: 14,
    imgNum: 6,
    isFlipped: false,
  },
  {
    number: 15,
    imgNum: 7,
    isFlipped: false,
  },
  {
    number: 16,
    imgNum: 8,
    isFlipped: false,
  },
]);

onMounted(() => {
  let currentIndex = randomNumbers.value.length,
    randomIndex;

  // While there remain elements to shuffle.
  while (currentIndex > 0) {
    // Pick a remaining element.
    randomIndex = Math.floor(Math.random() * currentIndex);
    currentIndex--;

    // And swap it with the current element.
    [randomNumbers.value[currentIndex], randomNumbers.value[randomIndex]] = [
      randomNumbers.value[randomIndex],
      randomNumbers.value[currentIndex],
    ];
  }
});
</script>

<style scoped>
.game-board {
  width: 304px;
  display: flex;
  flex-wrap: wrap;
  gap: 16px;
  padding-top: 32px;
}
</style>

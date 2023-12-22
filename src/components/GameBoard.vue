<template>
  <div class="game-board">
    <template v-for="(card, index) in cardArray" :key="index">
      <GameCard
        :cardNum="index + 1"
        :card="card"
        @click="flipCard(card.imgNum, index)"
      />
    </template>
  </div>
  <div class="finish-section">
    <button class="reset-game" @click="resetGame" v-if="isOver">
      شروع دوباره
    </button>
    <h2>{{ finishMsg }}</h2>
  </div>
</template>

<script setup>
import { ref, onMounted, reactive } from "vue";
import GameCard from "./GameCard.vue";

const props = defineProps({
  time: Number,
  count: Number,
  isOver: Boolean,
});

const emit = defineEmits([
  "startTimer",
  "decreaseCount",
  "resetTimeAndCount",
  "stopTimer",
  "isOverHandler",
]);

const firstCard = reactive({
  imgNum: null,
  index: null,
});
const secondCard = reactive({
  imgNum: null,
  index: null,
});
const cardArray = ref([]);
const timeout = ref(null);
const finishMsg = ref("");

onMounted(() => {
  generateCardArray();
});

const generateCardArray = () => {
  for (let i = 2; i < 18; i++) {
    cardArray.value.push({
      imgNum: Math.floor(i / 2),
      isFlipped: false,
    });
  }
  shuffleArray();
};

const flipCard = (num, index) => {
  if (props.time === 0 || props.count === 0) return;
  const selectedCard = cardArray.value[index];

  if (props.time === 120 && props.count === 40) emit("startTimer");

  if (!selectedCard.isFlipped && !secondCard.imgNum) {
    emit("decreaseCount");

    selectedCard.isFlipped = true;

    if (!firstCard.imgNum) {
      firstCard.imgNum = num;
      firstCard.index = index;
    } else {
      secondCard.imgNum = num;
      secondCard.index = index;
    }

    if (secondCard.imgNum) {
      if (firstCard.imgNum !== secondCard.imgNum) {
        timeout.value = setTimeout(() => {
          cancelFlipCard();
        }, 1000);
      } else {
        resetCardsState();
      }
    }
    checkWinOrLose();
  }
};

const checkWinOrLose = () => {
  const winCondition = cardArray.value.every((card) => card.isFlipped === true);
  if (winCondition) {
    emit("isOverHandler", true);
    finishMsg.value = "!تبریک، شما برنده شدید";
    emit("stopTimer");
  } else {
    if (props.time === 0 || props.count === 1) {
      emit("isOverHandler", true);
      finishMsg.value = "!متاسفم، شما باختید";
      emit("stopTimer");
    }
  }
};

const resetGame = () => {
  cardArray.value = [];
  resetCardsState();
  generateCardArray();
  emit("isOverHandler", false);
  emit("resetTimeAndCount");
  finishMsg.value = "";
};

const cancelFlipCard = () => {
  cardArray.value[firstCard.index].isFlipped = false;
  cardArray.value[secondCard.index].isFlipped = false;
  resetCardsState();
  clearTimeout(timeout.value);
};

const resetCardsState = () => {
  firstCard.imgNum = null;
  firstCard.index = null;
  secondCard.imgNum = null;
  secondCard.index = null;
};

const shuffleArray = () => {
  let currentIndex = cardArray.value.length,
    randomIndex;

  // While there remain elements to shuffle.
  while (currentIndex > 0) {
    // Pick a remaining element.
    randomIndex = Math.floor(Math.random() * currentIndex);
    currentIndex--;

    // And swap it with the current element.
    [cardArray.value[currentIndex], cardArray.value[randomIndex]] = [
      cardArray.value[randomIndex],
      cardArray.value[currentIndex],
    ];
  }
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

.finish-section {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-top: 1rem;
}

.reset-game {
  padding: 0.5rem 1rem;
  background-color: rgb(199, 199, 106);
  border: none;
  border-radius: 4px;
  font-size: larger;
  font-weight: bold;
}
</style>

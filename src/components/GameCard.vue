<template>
  <div class="card" :class="{ flipped: card.isFlipped }">
    <div class="card-inner">
      <div class="card-front">
        <button class="game-card">
          {{ cardNum }}
        </button>
      </div>
      <div class="card-back">
        <img
          class="card-img"
          :src="imgUrl"
          :alt="`product-${cardNum}`"
          width="80"
          height="80"
        />
      </div>
    </div>
  </div>
</template>

<script setup>
import { computed } from "vue";

const props = defineProps({
  cardNum: Number,
  card: {
    imgNum: Number,
    isFlipped: Boolean,
  },
});

const imgUrl = computed(
  () => `/assets/images/product-${props.card.imgNum}.jpg`
);
</script>

<style scoped>
.card {
  width: 80px;
  height: 80px;
  perspective: 1000px;
  cursor: pointer;
}

.card-inner {
  width: 100%;
  height: 100%;
  transform-style: preserve-3d;
  transition: transform 0.3s;
}

.card.flipped .card-inner {
  transform: rotateY(180deg);
}

.card-front,
.card-back {
  width: 100%;
  height: 100%;
  position: absolute;
  backface-visibility: hidden;
}

.card-back {
  transform: rotateY(180deg);
}
.game-card {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 80px;
  height: 80px;
  background-color: rgb(247, 65, 65);
  border-radius: 4px;
  color: white;
  border: none;
  transform: rotateY(0deg);
}

.card-img {
  border-radius: 4px;
}
</style>

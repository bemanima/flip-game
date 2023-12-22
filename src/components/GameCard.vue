<template>
  <img
    :src="imgUrl"
    :alt="`product-${cardNum}`"
    width="80"
    height="80"
    v-if="card.isFlipped"
  />
  <button class="game-card" v-else>
    {{ cardNum }}
  </button>
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
}
</style>

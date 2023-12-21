<template>
  <img
    :src="imgUrl"
    :alt="`product-${cardNum}`"
    width="64"
    height="64"
    v-if="isFlipped"
    :id="`image-${cardImg}`"
  />
  <button :id="cardImg" class="game-card" v-else>
    {{ cardNum }}
  </button>
</template>

<script setup>
import { computed, ref, inject } from "vue";
const props = defineProps({
  cardNum: Number,
  cardImg: Number,
  isFlipped: Boolean,
});

const imgUrl = computed(() => `/assets/images/product-${props.cardImg}.jpg`);
</script>

<style scoped>
.game-card {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 64px;
  height: 64px;
  background-color: rgb(247, 65, 65);
  border-radius: 4px;
  color: white;
  border: none;
}
</style>

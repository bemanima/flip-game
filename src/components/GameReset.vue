<template>
  <div class="finish-section">
    <button class="reset-game" @click="emit('reset')">شروع دوباره</button>
    <h2>{{ msg }}</h2>
  </div>
</template>

<script setup>
const emit = defineEmits(["reset"]);
const props = defineProps({
  msg: String,
});
</script>

<style scoped>
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

<template>
  <div class="info">
    <h2 class="time">زمان: {{ timerShow }}</h2>
    <h2 class="count-number">تعداد حرکت: {{ count }}</h2>
  </div>
</template>

<script setup>
import { computed } from "vue";

const props = defineProps({
  time: Number,
  count: Number,
});

const timerShow = computed(() => {
  const minutes = Math.floor(props.time / 60)
    .toString()
    .padStart(2, "0");
  const seconds = (props.time % 60).toString().padStart(2, "0");

  return `${minutes}:${seconds}`;
});
</script>

<style scoped>
.info {
  width: 368px;
  display: flex;
  justify-content: space-between;
}
</style>

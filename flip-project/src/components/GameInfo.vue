<template>
  <div class="info">
    <div class="time">زمان: {{ time }}</div>
    <div class="count-number">تعداد حرکت: {{ count }}</div>
  </div>
</template>

<script setup>
const props = defineProps({
  time: String,
  count: Number,
});
</script>

<style scoped></style>

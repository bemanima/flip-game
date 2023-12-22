<template>
  <div class="info">
    <h2 class="time">زمان: {{ time }}</h2>
    <h2 class="count-number">تعداد حرکت: {{ count }}</h2>
  </div>
</template>

<script setup>
const props = defineProps({
  time: String,
  count: Number,
});
</script>

<style scoped>
.info {
  width: 368px;
  display: flex;
  justify-content: space-between;
}
</style>

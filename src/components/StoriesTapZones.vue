<template>
  <div class="stories-tap-zones">
    <div class="stories-tap-zone stories-tap-zone--left" @click="$emit('prev')"></div>
    <div class="stories-tap-zone stories-tap-zone--right" @click="$emit('next')"></div>
  </div>
</template>

<script setup lang="ts">
defineEmits<{
  prev: []
  next: []
}>()
</script>

<style lang="scss" scoped>
.stories-tap-zones {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 1;
  display: flex;
}

.stories-tap-zone {
  flex: 1 1 0;
  height: 100%;
}
</style>

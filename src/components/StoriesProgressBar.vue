<template>
  <div class="stories-progress">
    <div class="story-bar" v-for="(segment, i) in count" :key="i">
      <div
        class="story-filled"
        :style="{ width: i < current ? '100%' : i > current ? '0%' : progress + '%' }"
      ></div>
    </div>
  </div>
</template>

<script setup lang="ts">
defineProps<{
  count: number
  current: number
  progress: number
}>()
</script>

<style lang="scss" scoped>
.stories-progress {
  display: flex;
  width: 100%;
  gap: rem(4px);

  @include for-desktop {
    gap: player-adapt(4px);
  }
}

.story-bar {
  flex: 1;
  background: $color-progress-empty;
  height: rem(4px);
  border-radius: rem(4px);
  overflow: hidden;

  @include for-desktop {
    height: player-adapt(4px);
    border-radius: player-adapt(4px);
  }
}

.story-filled {
  background: $color-progress-filled;
  height: 100%;
  border-radius: rem(4px);
  transition: width 0.2s linear;

  @include for-desktop {
    border-radius: player-adapt(4px);
  }
}
</style>

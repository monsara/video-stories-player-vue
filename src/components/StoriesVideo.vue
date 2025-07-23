<template>
  <video
    class="stories-video"
    :ref="setVideoRef"
    @ended="$emit('ended')"
    @timeupdate="$emit('timeupdate', $event)"
    @loadedmetadata="$emit('loadedmetadata')"
    playsinline
    reload="metadata"
    :autoplay="autoplay"
    :muted="muted"
    width="430"
    height="767"
    v-bind="poster ? { poster } : {}"
  >
    <source :src="srcAv1" type="video/av1" />
    <source :src="srcWebm" type="video/webm" />
    <source :src="srcMp4" type="video/mp4" />
    Ваш браузер не підтримує відео.
  </video>
</template>

<script setup lang="ts">
import type { ComponentPublicInstance } from 'vue'

defineProps<{
  srcAv1: string
  srcWebm: string
  srcMp4: string
  autoplay: boolean
  muted: boolean
  setVideoRef?: (el: Element | ComponentPublicInstance | null) => void
  poster?: string
}>()

defineEmits(['ended', 'timeupdate', 'loadedmetadata'])
</script>

<style lang="scss" scoped>
.stories-video {
  display: block;
  width: 100%;
  height: 100%;
  object-fit: cover;
  opacity: 0;
  transition: opacity 0.2s;

  @include for-desktop {
    border-radius: $border-radius-player;
  }
}

.stories-video.video-visible {
  opacity: 1;
}
</style>

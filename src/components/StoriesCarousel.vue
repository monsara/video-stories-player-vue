<template>
  <div class="stories-carousel">
    <StoriesNavs v-if="isDesktop" @prev="$emit('prev')" @next="$emit('next')" />
    <StoriesTapZones v-if="!isDesktop" @prev="$emit('handlePrev')" @next="$emit('handleNext')" />
    <StoriesVideo
      v-if="videos[currentIndex]"
      :key="videos[currentIndex].srcMp4"
      :srcAv1="videos[currentIndex].srcAv1"
      :srcWebm="videos[currentIndex].srcWebm"
      :srcMp4="videos[currentIndex].srcMp4"
      :autoplay="!paused"
      :muted="isMuted"
      :setVideoRef
      :poster="videos[currentIndex].poster"
      @ended="$emit('next')"
      @timeupdate="$emit('timeupdate', $event)"
      @loadedmetadata="$emit('loadedmetadata')"
      :class="{ 'video-visible': isVideoReady }"
    />
  </div>
</template>

<script setup lang="ts">
import type { ComponentPublicInstance } from 'vue'
import StoriesNavs from './StoriesNavs.vue'
import StoriesTapZones from './StoriesTapZones.vue'
import StoriesVideo from './StoriesVideo.vue'

defineProps<{
  videos: Array<{
    srcAv1: string
    srcWebm: string
    srcMp4: string
    poster: string
    subtitle: string
  }>
  currentIndex: number
  paused: boolean
  isMuted: boolean
  isDesktop: boolean
  isVideoReady: boolean
  setVideoRef: (el: Element | ComponentPublicInstance | null) => void
}>()

defineEmits(['next', 'prev', 'handlePrev', 'handleNext', 'timeupdate', 'loadedmetadata'])
</script>

<style lang="scss" scoped>
.stories-carousel {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  width: 100%;
  height: 100%;
  background: $color-bg-story;

  @include for-desktop {
    border-radius: $border-radius-player;
  }
}
</style>

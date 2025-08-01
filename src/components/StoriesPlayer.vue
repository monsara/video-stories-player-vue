<template>
  <div ref="playerRef" class="stories-player">
    <StoriesHeader
      :videos
      :currentIndex
      :progress
      :isDesktop
      :paused
      :isMuted
      @play="onPlay"
      @pause="onPause"
      @toggle-mute="toggleMute"
    />
    <StoriesCarousel
      :videos
      :currentIndex
      :paused
      :isMuted
      :isDesktop
      :isVideoReady
      :setVideoRef
      @next="onNext"
      @prev="onPrev"
      @handlePrev="handlePrev"
      @handleNext="handleNext"
      @timeupdate="onTimeUpdate"
      @loadedmetadata="onLoadedMetadata"
    />
  </div>
</template>

<script setup lang="ts">
import { ref, computed, onMounted, onUnmounted } from 'vue'

import type { ComponentPublicInstance } from 'vue'
import { onLongPress } from '@vueuse/core'

import StoriesCarousel from './StoriesCarousel.vue'
import StoriesHeader from './StoriesHeader.vue'

const props = defineProps<{
  videos: Array<{
    srcAv1: string
    srcWebm: string
    srcMp4: string
    poster: string
    subtitle: string
  }>
}>()

const videos = props.videos

const currentIndex = ref(0)
const progress = ref(0)
const paused = ref(false)
const isMuted = ref(true)
const isVideoReady = ref(false)
const wasLongPress = ref(false)

const windowWidth = ref(window.innerWidth)
const videoRefs = ref<HTMLVideoElement[]>([])
const playerRef = ref<HTMLElement | null>(null)

const isDesktop = computed(() => windowWidth.value >= 768)

onMounted(() => {
  window.addEventListener('resize', handleResize)
})

onUnmounted(() => {
  window.removeEventListener('resize', handleResize)
})

const setVideoRef = (el: Element | ComponentPublicInstance | null) => {
  if (el instanceof HTMLVideoElement) {
    videoRefs.value[currentIndex.value] = el
  }
}

const onPrev = () => {
  if (currentIndex.value > 0) {
    currentIndex.value--
    progress.value = 0
    isVideoReady.value = false
  }
}

const onNext = () => {
  if (currentIndex.value < videos.length - 1) {
    currentIndex.value++
    progress.value = 0
    isVideoReady.value = false
  }
}

const onPlay = () => {
  paused.value = false
  const currentVideo = videoRefs.value[currentIndex.value]
  if (currentVideo) {
    currentVideo.muted = isMuted.value
    currentVideo.play()
  }
}

const onPause = () => {
  paused.value = true
  const currentVideo = videoRefs.value[currentIndex.value]
  if (currentVideo) currentVideo.pause()
}

const onLoadedMetadata = () => {
  progress.value = 0
  isVideoReady.value = true
}

const onTimeUpdate = (e: Event) => {
  const video = e.target as HTMLVideoElement
  progress.value = (video.currentTime / video.duration) * 100
}

const toggleMute = () => {
  isMuted.value = !isMuted.value
  videoRefs.value.forEach((v) => {
    if (v) v.muted = isMuted.value
  })
}

const handleResize = () => {
  windowWidth.value = window.innerWidth
}

const handleLongPress = (e: PointerEvent) => {
  onPause()
  wasLongPress.value = true
}

onLongPress(playerRef, handleLongPress, {
  delay: 500,
  modifiers: { prevent: true },
  onMouseUp: () => {
    if (paused.value) onPlay()
  },
})

const handlePrev = () => {
  if (wasLongPress.value) {
    wasLongPress.value = false
    return
  }
  onPrev()
}

const handleNext = () => {
  if (wasLongPress.value) {
    wasLongPress.value = false
    return
  }
  onNext()
}
</script>

<style lang="scss" scoped>
.stories-player {
  position: fixed;
  width: 100%;
  height: 100dvh;
  margin: 0 auto;
  background: $color-bg-dark;
  position: relative;
  padding: rem(16px);
  user-select: none;
  -webkit-user-select: none;
  -webkit-touch-callout: none;
  -webkit-tap-highlight-color: transparent;

  @include for-desktop {
    width: auto;
    height: $player-height;
    padding: player-adapt(16px);
    aspect-ratio: 430 / 768;
    border-radius: $border-radius-player;
  }
}
</style>

<template>
  <div class="stories-header">
    <StoriesProgressBar :count="videos.length" :current="currentIndex" :progress="progress" />
    <div class="story-details">
      <div class="stories-logo">
        <img src="/images/logo_img.svg" alt="Logo" class="logo-img" />
      </div>
      <div class="story-text">
        <div class="story-title">Upstars</div>
        <div class="story-subtitle" ref="subtitleRef"></div>
      </div>
      <StoriesControls
        :isDesktop
        :paused
        :isMuted
        @play="$emit('play')"
        @pause="$emit('pause')"
        @toggle-mute="$emit('toggle-mute')"
      />
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, watch, onMounted, nextTick } from 'vue'
import gsap from 'gsap'
import StoriesProgressBar from './StoriesProgressBar.vue'
import StoriesControls from './StoriesControls.vue'

interface Video {
  srcAv1: string
  srcWebm: string
  srcMp4: string
  poster: string
  subtitle: string
}

const props = defineProps<{
  videos: Array<Video>
  currentIndex: number
  progress: number
  isDesktop: boolean
  paused: boolean
  isMuted: boolean
}>()

defineEmits(['play', 'pause', 'toggle-mute'])

const subtitleRef = ref<HTMLElement | null>(null)

const typewriterEffect = (text: string) => {
  if (!subtitleRef.value) return
  gsap.killTweensOf(subtitleRef.value)
  subtitleRef.value.textContent = ''
  let i = 0
  const type = () => {
    if (!subtitleRef.value) return
    if (i <= text.length) {
      subtitleRef.value.textContent = text.slice(0, i)
      i++
      gsap.delayedCall(0.04, type)
    }
  }
  type()
}

watch(
  () => props.currentIndex,
  async () => {
    await nextTick()
    typewriterEffect(props.videos[props.currentIndex].subtitle)
  },
)

onMounted(() => {
  typewriterEffect(props.videos[props.currentIndex].subtitle)
})
</script>

<style lang="scss" scoped>
.stories-header {
  position: relative;
  z-index: 2;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: center;
  gap: rem(8px);

  @include for-desktop {
    gap: player-adapt(8px);
  }
}

.stories-logo {
  width: rem(40px);
  height: rem(40px);
  flex-shrink: 0;

  @include for-desktop {
    height: player-adapt(44px);
    width: player-adapt(44px);
  }
}

.logo-img {
  width: 100%;
  height: 100%;
  object-fit: contain;
}

.story-details {
  position: relative;
  display: flex;
  width: 100%;
  align-items: center;
  gap: rem(16px);

  @include for-desktop {
    gap: player-adapt(16px);
  }
}

.story-text {
  display: flex;
  flex-grow: 1;
  flex-direction: column;
  align-items: start;
  justify-content: center;
  font-family: $font-family-main;
  font-style: normal;
  font-weight: 700;
  color: $color-text-main;
  overflow: hidden;
}

.story-title {
  width: 100%;
  font-size: rem(14px);
  line-height: 170%;
  font-weight: inherit;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;

  @include for-desktop {
    font-size: player-adapt(14px);
  }
}

.story-subtitle {
  width: 100%;
  height: rem(12px);
  font-size: rem(12px);
  line-height: 130%;
  font-weight: inherit;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;

  @include for-desktop {
    height: player-adapt(12px);
    font-size: player-adapt(12px);
  }
}
</style>

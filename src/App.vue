<script setup lang="ts">
import StoriesPlayer from './components/StoriesPlayer.vue'

interface Video {
  srcAv1: string
  srcWebm: string
  srcMp4: string
  poster: string
  subtitle: string
}

const videos: Video[] = [
  {
    srcAv1: '/videos/av1/video1.av1',
    srcWebm: '/videos/webm/video1.webm',
    srcMp4: '/videos/h264/video1.mp4',
    poster: '/posters/video1.jpg',
    subtitle: 'Бенефіти',
  },
  {
    srcAv1: '/videos/av1/video2.av1',
    srcWebm: '/videos/webm/video2.webm',
    srcMp4: '/videos/h264/video2.mp4',
    poster: '/posters/video2.jpg',
    subtitle: 'Домашні улюбленці',
  },
  {
    srcAv1: '/videos/av1/video3.av1',
    srcWebm: '/videos/webm/video3.webm',
    srcMp4: '/videos/h264/video3.mp4',
    poster: '/posters/video3.jpg',
    subtitle: 'UPCHARITY 2.0',
  },
]
</script>

<template>
  <StoriesPlayer :videos="videos" />
</template>

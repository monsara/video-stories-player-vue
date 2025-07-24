<template>
  <div class="stories-controls">
    <button v-if="isDesktop && paused" class="btn-play btn-control" @click="$emit('play')">
      <PlayIcon class="icon" />
    </button>
    <button v-if="isDesktop && !paused" class="btn-pause btn-control" @click="$emit('pause')">
      <PauseIcon class="icon" />
    </button>
    <button class="btn-mute btn-control" @click="$emit('toggle-mute')">
      <MuteIcon v-if="isMuted" class="icon" />
      <UnmuteIcon v-else class="icon" />
    </button>
    <a class="btn-close btn-control" href="https://www.upstars.com/" rel="noopener noreferrer">
      <CloseIcon class="icon" />
    </a>
  </div>
</template>

<script setup lang="ts">
import PlayIcon from '@/assets/icons/play.svg'
import PauseIcon from '@/assets/icons/pause.svg'
import CloseIcon from '@/assets/icons/close.svg'
import MuteIcon from '@/assets/icons/muted.svg'
import UnmuteIcon from '@/assets/icons/unmuted.svg'

defineProps<{
  isDesktop: boolean
  paused: boolean
  isMuted: boolean
}>()

defineEmits(['play', 'pause', 'toggle-mute'])
</script>

<style lang="scss" scoped>
.stories-controls {
  display: flex;
  justify-content: end;
  align-items: center;
  gap: rem(8px);

  @include for-desktop {
    gap: player-adapt(8px);
  }
}

.btn-control {
  display: flex;
  width: rem(24px);
  height: rem(24px);
  flex-shrink: 0;
  align-items: center;
  justify-content: center;
  color: $color-white;
  fill: $color-white;

  @include for-desktop {
    width: player-adapt(24px);
    height: player-adapt(24px);
  }

  .icon {
    filter: drop-shadow(0px rem(4px) rem(4px) rgba(0, 0, 0, 0.25));
  }
}

.btn-close {
  .icon {
    width: rem(16px);
    height: rem(16px);

    @include for-desktop {
      width: player-adapt(16px);
      height: player-adapt(16px);
    }
  }
}

.btn-pause {
  .icon {
    width: rem(10px);
    height: rem(18px);
  }

  @include for-desktop {
    .icon {
      width: player-adapt(10px);
      height: player-adapt(18px);
    }
  }
}

.btn-play {
  .icon {
    width: rem(14px);
    height: rem(18px);
  }

  @include for-desktop {
    .icon {
      width: player-adapt(14px);
      height: player-adapt(18px);
    }
  }
}

.btn-mute,
.btn-unmute {
  .icon {
    width: rem(32px);
    height: rem(32px);
  }

  @include for-desktop {
    .icon {
      width: player-adapt(32px);
      height: player-adapt(32px);
    }
  }
}
</style>

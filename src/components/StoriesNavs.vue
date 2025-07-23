<template>
  <div class="stories-navs">
    <button class="stories-nav-btn stories-nav-btn-prev" @click="$emit('prev')">
      <ArrowIcon class="icon" />
    </button>
    <button class="stories-nav-btn stories-nav-btn-next" @click="$emit('next')">
      <ArrowIcon class="icon" />
    </button>
  </div>
</template>

<script setup lang="ts">
import ArrowIcon from '@/assets/icons/arrow.svg'

defineEmits(['prev', 'next'])
</script>

<style lang="scss" scoped>
.stories-navs {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  pointer-events: none;
  z-index: 2;
}

.stories-nav-btn {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  display: flex;
  width: rem(40px);
  height: rem(40px);
  justify-content: center;
  align-items: center;
  background: $color-btn-nav;
  border-radius: 50%;
  color: $color-accent;
  pointer-events: auto;
  transition: background 0.2s;

  @include for-desktop {
    width: player-adapt(40px);
    height: player-adapt(40px);
  }

  .icon {
    width: rem(8px);
    height: rem(14px);
    fill: $color-white;
    display: block;

    @include for-desktop {
      width: player-adapt(8px);
      height: player-adapt(14px);
    }
  }
}

.stories-nav-btn-prev {
  left: rem(-64px);

  @include for-desktop {
    left: player-adapt(-64px);
  }

  .icon {
    transform: rotate(180deg);
  }
}

.stories-nav-btn-next {
  right: rem(-64px);

  @include for-desktop {
    right: player-adapt(-64px);
  }
}

.stories-nav-btn-prev:hover,
.stories-nav-btn-next:hover {
  background: rgba($color-btn-nav, 0.6);
}
</style>

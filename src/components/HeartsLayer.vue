<script setup>
defineProps({
  hearts: {
    type: Array,
    required: true,
  },
});
</script>

<template>
  <div class="hearts" aria-hidden="true">
    <span
      v-for="(heart, index) in hearts"
      :key="index"
      class="heart"
      :style="{
        left: heart.left,
        fontSize: heart.size,
        animationDelay: heart.delay,
        animationDuration: heart.duration,
        opacity: heart.opacity,
      }"
    >
      ❤
    </span>
  </div>
</template>

<style scoped>
.hearts {
  position: absolute;
  inset: 0;
  overflow: hidden;
  z-index: 0;
  pointer-events: none;
}

.heart {
  position: absolute;
  top: -10%;
  color: var(--color-heart);
  animation-name: fall;
  animation-timing-function: linear;
  animation-iteration-count: infinite;
  text-shadow: 0 6px 12px var(--color-heart-shadow);
}
</style>

<template>
  <div class="glow"></div>
</template>

<style scoped>
.glow {
  position: absolute;
  width: 420px;
  height: 420px;
  background: radial-gradient(circle, var(--color-glow), transparent 65%);
  top: -120px;
  left: -80px;
  filter: blur(2px);
  animation: float 10s ease-in-out infinite;
}
</style>

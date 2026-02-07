<script setup>
defineProps({
  variant: {
    type: String,
    required: true,
  },
  title: {
    type: String,
    required: true,
  },
  subtitle: {
    type: String,
    default: "",
  },
  emojiTop: {
    type: String,
    required: true,
  },
  emojiBottom: {
    type: String,
    required: true,
  },
});
</script>

<template>
  <section class="no-screen" :class="variant">
    <div class="no-emoji">{{ emojiTop }}</div>
    <h2>{{ title }}</h2>
    <p v-if="subtitle">{{ subtitle }}</p>
    <div class="no-emoji">{{ emojiBottom }}</div>
  </section>
</template>

<style scoped>
.no-screen {
  margin-top: 24px;
  text-align: center;
  padding: 28px 20px 32px;
  border-radius: 24px;
  color: #ffffff;
  animation: fade-up 0.6s ease;
}

.no-screen h2 {
  font-size: clamp(22px, 4vw, 30px);
  margin: 12px 0;
  font-weight: 800;
}

.no-screen p {
  margin: 0;
  font-size: 18px;
  font-weight: 600;
}

.no-screen.sad {
  background: linear-gradient(135deg, var(--color-no-sad-1), var(--color-no-sad-2));
  box-shadow: 0 18px 40px var(--color-no-sad-shadow);
}

.no-screen.tease {
  background: linear-gradient(135deg, var(--color-no-tease-1), var(--color-no-tease-2));
  box-shadow: 0 18px 40px var(--color-no-tease-shadow);
}

.no-emoji {
  font-size: clamp(40px, 7vw, 64px);
  margin: 10px 0;
}
</style>

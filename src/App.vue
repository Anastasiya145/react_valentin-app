<script setup>
import { computed, nextTick, onMounted, onUnmounted, ref } from "vue";
import photoUrl from "./my-photo.jpg";
import HeartsLayer from "./components/HeartsLayer.vue";
import GlowOrb from "./components/GlowOrb.vue";
import ValentineHero from "./components/ValentineHero.vue";
import AskSection from "./components/AskSection.vue";
import AcceptedSection from "./components/AcceptedSection.vue";
import NoScreen from "./components/NoScreen.vue";

const view = ref("ask");
const timers = [];

const showHero = computed(() => view.value === "ask" || view.value === "yes");

const hearts = Array.from({ length: 16 }, () => ({
  left: `${Math.floor(Math.random() * 90) + 5}%`,
  size: `${Math.floor(Math.random() * 14) + 12}px`,
  delay: `${(Math.random() * 6).toFixed(2)}s`,
  duration: `${Math.floor(Math.random() * 10) + 10}s`,
  opacity: (Math.random() * 0.5 + 0.35).toFixed(2),
}));

const clearTimers = () => {
  timers.splice(0).forEach((id) => clearTimeout(id));
};

const goToAsk = async () => {
  view.value = "ask";
  await nextTick();
};

const handleNoClick = () => {
  clearTimers();
  view.value = "no1";

  timers.push(
    setTimeout(() => {
      view.value = "no2";
    }, 2200),
  );

  timers.push(
    setTimeout(() => {
      goToAsk();
    }, 6000),
  );
};

const handleYesClick = () => {
  view.value = "yes";
};

onMounted(async () => {
  await nextTick();
});

onUnmounted(() => {
  clearTimers();
});
</script>

<template>
  <div class="page">
    <HeartsLayer :hearts="hearts" />
    <GlowOrb />
    <main class="card" :class="{ fullscreen: view === 'no1' || view === 'no2' }">
      <ValentineHero v-if="showHero" />

      <AskSection v-if="view === 'ask'" :on-yes="handleYesClick" :on-no="handleNoClick" />

      <AcceptedSection v-else-if="view === 'yes'" :photo-url="photoUrl" />

      <NoScreen
        v-else-if="view === 'no1'"
        variant="sad"
        title="Tu es sérieux ???"
        emoji-top="😿"
        emoji-bottom="💔"
      />

      <NoScreen
        v-else
        variant="tease"
        title="HAHAHA !!! Comme tu t'es fait avoir !"
        subtitle="Tu pensais que je te laisserais cette option ?!"
        emoji-top="😂😂😂"
        emoji-bottom="👉👉"
      />
    </main>
  </div>
</template>

<style scoped>
.page {
  min-height: 100vh;
  min-height: 100svh;
  display: grid;
  place-items: center;
  background:
    radial-gradient(circle at top right, var(--color-page-grad-1) 0%, transparent 55%),
    radial-gradient(circle at 10% 20%, var(--color-page-grad-2) 0%, transparent 50%),
    linear-gradient(
      135deg,
      var(--color-page-grad-3) 0%,
      var(--color-page-grad-4) 50%,
      var(--color-page-grad-5) 100%
    );
  position: relative;
  overflow: hidden;
  padding: calc(32px + env(safe-area-inset-top)) calc(16px + env(safe-area-inset-right))
    calc(32px + env(safe-area-inset-bottom)) calc(16px + env(safe-area-inset-left));
}

.card {
  width: min(520px, 92vw);
  background: var(--color-card-bg);
  border-radius: 32px;
  padding: 36px 32px 32px;
  box-sizing: border-box;
  box-shadow: 0 18px 45px var(--color-card-shadow);
  backdrop-filter: blur(12px);
  position: relative;
  z-index: 1;
  border: 1px solid var(--color-card-border);
  animation: pop-in 0.8s ease;
}

.card.fullscreen {
  width: min(720px, 96vw);
  min-height: 360px;
  padding: 28px;
}

@media (max-width: 520px) {
  .page {
    padding: calc(24px + env(safe-area-inset-top)) calc(12px + env(safe-area-inset-right))
      calc(24px + env(safe-area-inset-bottom)) calc(12px + env(safe-area-inset-left));
  }

  .card {
    width: calc(100% - 24px);
    max-width: 100%;
    margin: 0 auto;
    padding: 26px 20px 24px;
  }

  .card.fullscreen {
    width: 100%;
    max-width: 100%;
    min-height: 320px;
  }
}
</style>

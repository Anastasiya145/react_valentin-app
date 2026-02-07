<script setup>
import { computed, nextTick, onMounted, onUnmounted, reactive, ref } from "vue";
import photoUrl from "./my-photo.jpg";
import HeartsLayer from "./components/HeartsLayer.vue";
import GlowOrb from "./components/GlowOrb.vue";
import ValentineHero from "./components/ValentineHero.vue";
import AskSection from "./components/AskSection.vue";
import AcceptedSection from "./components/AcceptedSection.vue";
import NoScreen from "./components/NoScreen.vue";

const view = ref("ask");
const stageRef = ref(null);
const noBtnRef = ref(null);
const noPos = reactive({ x: 0, y: 0 });
const timers = [];

const showHero = computed(() => view.value === "ask" || view.value === "yes");

const hearts = Array.from({ length: 16 }, () => ({
  left: `${Math.floor(Math.random() * 90) + 5}%`,
  size: `${Math.floor(Math.random() * 14) + 12}px`,
  delay: `${(Math.random() * 6).toFixed(2)}s`,
  duration: `${Math.floor(Math.random() * 10) + 10}s`,
  opacity: (Math.random() * 0.5 + 0.35).toFixed(2),
}));

const moveNoButton = () => {
  const stage = stageRef.value;
  const noBtn = noBtnRef.value;
  if (!stage || !noBtn) return;

  const stageRect = stage.getBoundingClientRect();
  const btnRect = noBtn.getBoundingClientRect();
  const padding = 12;

  const maxX = Math.max(padding, stageRect.width - btnRect.width - padding);
  const maxY = Math.max(padding, stageRect.height - btnRect.height - padding);

  noPos.x = Math.floor(Math.random() * maxX);
  noPos.y = Math.floor(Math.random() * maxY);
};

const clearTimers = () => {
  timers.splice(0).forEach((id) => clearTimeout(id));
};

const goToAsk = async () => {
  view.value = "ask";
  await nextTick();
  moveNoButton();
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
  moveNoButton();
  window.addEventListener("resize", moveNoButton);
});

onUnmounted(() => {
  window.removeEventListener("resize", moveNoButton);
  clearTimers();
});
</script>

<template>
  <div class="page">
    <HeartsLayer :hearts="hearts" />
    <GlowOrb />
    <main class="card" :class="{ fullscreen: view === 'no1' || view === 'no2' }">
      <ValentineHero v-if="showHero" />

      <AskSection
        v-if="view === 'ask'"
        :stage-ref="stageRef"
        :no-btn-ref="noBtnRef"
        :no-pos="noPos"
        :on-yes="handleYesClick"
        :on-no="handleNoClick"
        :on-move-no="moveNoButton"
      />

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
  padding: 32px 16px;
}

.card {
  width: min(520px, 92vw);
  background: var(--color-card-bg);
  border-radius: 32px;
  padding: 36px 32px 32px;
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
  .card {
    padding: 28px 24px;
  }
}
</style>

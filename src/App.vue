<script setup>
import { onMounted, onUnmounted, nextTick, reactive, ref } from "vue";
import photoUrl from "./my-photo.jpg";

const accepted = ref(false);
const stageRef = ref(null);
const noBtnRef = ref(null);
const noPos = reactive({ x: 0, y: 0 });

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

onMounted(async () => {
  await nextTick();
  moveNoButton();
  window.addEventListener("resize", moveNoButton);
});

onUnmounted(() => {
  window.removeEventListener("resize", moveNoButton);
});
</script>

<template>
  <div class="page">
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
    <div class="glow"></div>
    <main class="card" :class="{ accepted }">
      <header class="hero">
        <div class="badge">14.02</div>
        <h1>Mon amour,</h1>
        <p>veux-tu être ma valentine ?</p>
      </header>

      <section v-if="!accepted" class="content">
        <div ref="stageRef" class="button-stage">
          <button class="btn yes" type="button" @click="accepted = true">Oui, je veux ❤️</button>
          <button
            ref="noBtnRef"
            class="btn no"
            type="button"
            :style="{ left: `${noPos.x}px`, top: `${noPos.y}px` }"
            @mouseenter="moveNoButton"
            @pointerenter="moveNoButton"
            @touchstart.prevent="moveNoButton"
          >
            Non
          </button>
        </div>
        <p class="note">Attrape le bouton « Non » si tu peux 😏</p>
      </section>

      <section v-else class="accepted-state">
        <div class="confetti">🎉</div>
        <h2>Yesss !</h2>
        <p>Je t’attends le 14 février pour le rendez-vous le plus romantique.</p>
        <div class="photo-frame">
          <img :src="photoUrl" alt="Notre photo" />
        </div>
        <div class="signature">Ta valentine 💌</div>
      </section>
    </main>
  </div>
</template>

<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Manrope:wght@400;600;800&family=Playfair+Display:wght@600;700&display=swap");

:global(body) {
  margin: 0;
  font-family: "Manrope", system-ui, sans-serif;
  color: #2a2233;
  background: #f7dbe7;
}

:global(#app) {
  min-height: 100vh;
}

.page {
  min-height: 100vh;
  display: grid;
  place-items: center;
  background:
    radial-gradient(circle at top right, #ffd6e6 0%, transparent 55%),
    radial-gradient(circle at 10% 20%, #ffe8f2 0%, transparent 50%),
    linear-gradient(135deg, #fce6ec 0%, #f6d0dc 50%, #f3bcd0 100%);
  position: relative;
  overflow: hidden;
  padding: 32px 16px;
}

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
  color: rgba(216, 27, 96, 0.55);
  animation-name: fall;
  animation-timing-function: linear;
  animation-iteration-count: infinite;
  text-shadow: 0 6px 12px rgba(216, 27, 96, 0.2);
}

.glow {
  position: absolute;
  width: 420px;
  height: 420px;
  background: radial-gradient(circle, rgba(255, 180, 210, 0.75), transparent 65%);
  top: -120px;
  left: -80px;
  filter: blur(2px);
  animation: float 10s ease-in-out infinite;
}

.card {
  width: min(520px, 92vw);
  background: rgba(255, 255, 255, 0.88);
  border-radius: 32px;
  padding: 36px 32px 32px;
  box-shadow: 0 18px 45px rgba(194, 72, 122, 0.2);
  backdrop-filter: blur(12px);
  position: relative;
  z-index: 1;
  border: 1px solid rgba(255, 255, 255, 0.6);
  animation: pop-in 0.8s ease;
}

.hero h1 {
  font-family: "Playfair Display", serif;
  font-size: clamp(28px, 4vw, 40px);
  margin: 12px 0 6px;
}

.hero p {
  font-size: 18px;
  margin: 0;
  color: #5b475b;
}

.badge {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  padding: 6px 14px;
  border-radius: 999px;
  font-size: 14px;
  font-weight: 700;
  letter-spacing: 0.12em;
  background: #ffb3c9;
  color: #7d1e46;
}

.content {
  margin-top: 28px;
}

.button-stage {
  position: relative;
  height: 180px;
  border-radius: 24px;
  background: linear-gradient(135deg, #fff1f6, #ffe4ef);
  border: 1px dashed rgba(208, 110, 153, 0.35);
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
}

.btn {
  border: none;
  font-family: "Manrope", sans-serif;
  font-weight: 700;
  border-radius: 999px;
  padding: 12px 26px;
  cursor: pointer;
  transition:
    transform 0.2s ease,
    box-shadow 0.2s ease;
}

.btn.yes {
  background: linear-gradient(135deg, #f06292, #d81b60);
  color: white;
  box-shadow: 0 10px 24px rgba(216, 27, 96, 0.3);
}

.btn.yes:hover {
  transform: translateY(-2px) scale(1.02);
}

.btn.no {
  position: absolute;
  background: #ffffff;
  color: #a43f63;
  border: 2px solid #f4b3c8;
  box-shadow: 0 8px 16px rgba(164, 63, 99, 0.15);
  transition:
    left 0.2s ease,
    top 0.2s ease;
}

.note {
  margin: 16px 0 0;
  font-size: 14px;
  color: #7b5a6e;
  text-align: center;
}

.accepted-state {
  margin-top: 24px;
  text-align: center;
  animation: fade-up 0.6s ease;
}

.accepted-state h2 {
  font-family: "Playfair Display", serif;
  font-size: clamp(26px, 4vw, 36px);
  margin: 12px 0;
}

.accepted-state p {
  font-size: 18px;
  margin: 0 0 12px;
  color: #5c3d52;
}

.photo-frame {
  margin: 12px auto 16px;
  width: min(280px, 70vw);
  border-radius: 22px;
  overflow: hidden;
  border: 4px solid #ffd1e1;
  box-shadow: 0 14px 28px rgba(177, 71, 109, 0.2);
}

.photo-frame img {
  width: 100%;
  height: auto;
  display: block;
}

.confetti {
  font-size: 56px;
  animation: bounce 1.2s ease infinite;
}

.signature {
  font-weight: 600;
  color: #b23e68;
}

@keyframes pop-in {
  from {
    transform: translateY(18px) scale(0.98);
    opacity: 0;
  }
  to {
    transform: translateY(0) scale(1);
    opacity: 1;
  }
}

@keyframes fall {
  0% {
    transform: translateY(-10vh) rotate(0deg);
  }
  100% {
    transform: translateY(110vh) rotate(35deg);
  }
}

@keyframes fade-up {
  from {
    transform: translateY(12px);
    opacity: 0;
  }
  to {
    transform: translateY(0);
    opacity: 1;
  }
}

@keyframes float {
  0%,
  100% {
    transform: translate(0, 0);
  }
  50% {
    transform: translate(24px, 12px);
  }
}

@keyframes bounce {
  0%,
  100% {
    transform: translateY(0);
  }
  50% {
    transform: translateY(-12px);
  }
}

@media (max-width: 520px) {
  .card {
    padding: 28px 24px;
  }

  .hero h1 {
    font-size: 32px;
  }

  .hero p {
    font-size: 19px;
  }

  .badge {
    font-size: 15px;
    padding: 8px 16px;
  }

  .button-stage {
    height: 190px;
  }

  .btn {
    padding: 14px 24px;
    font-size: 16px;
  }

  .note {
    font-size: 15px;
  }

  .accepted-state p {
    font-size: 19px;
  }
}
</style>

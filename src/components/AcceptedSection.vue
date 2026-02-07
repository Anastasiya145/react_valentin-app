<script setup>
defineProps({
  photoUrl: {
    type: String,
    required: true,
  },
});
</script>

<template>
  <section class="accepted-state">
    <div class="confetti">🎉</div>
    <h2>Ouiiii !</h2>
    <p>Je t’attends le 14 février pour le rendez-vous le plus romantique.</p>
    <p class="meet"><strong>📍 Lieu : 20 rue Michel Tournier, 14120 MONDEVILLE</strong></p>
    <p class="meet"><strong>⏰ Heure : 14h30</strong></p>
    <div class="photo-frame">
      <img :src="photoUrl" alt="Notre photo" />
    </div>
    <div class="signature">Ta valentine 💌</div>
  </section>
</template>

<style scoped>
.accepted-state {
  margin-top: 24px;
  text-align: center;
  animation: fade-up 0.6s ease;
}

.accepted-state h2 {
  font-family: var(--font-serif);
  font-size: clamp(26px, 4vw, 36px);
  margin: 12px 0;
}

.accepted-state p {
  font-size: 18px;
  margin: 0 0 12px;
  color: var(--color-text-muted-strong);
}

.meet {
  margin-top: 6px;
}

.photo-frame {
  margin: 12px auto 16px;
  width: min(280px, 70vw);
  border-radius: 22px;
  overflow: hidden;
  border: 4px solid var(--color-photo-border);
  box-shadow: 0 14px 28px var(--color-photo-shadow);
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
  color: var(--color-signature);
}

@media (max-width: 520px) {
  .accepted-state p {
    font-size: 19px;
  }
}
</style>

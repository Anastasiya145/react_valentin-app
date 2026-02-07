<template>
  <header class="hero">
    <div class="badge">14.02</div>
    <h1>Mon amour,</h1>
    <p>veux-tu être mon valentin ?</p>
  </header>
</template>

<style scoped>
.hero h1 {
  font-family: var(--font-serif);
  font-size: clamp(28px, 4vw, 40px);
  margin: 12px 0 6px;
}

.hero p {
  font-size: 18px;
  margin: 0;
  color: var(--color-text-muted);
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
  background: var(--color-badge-bg);
  color: var(--color-badge-text);
}

@media (max-width: 520px) {
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
}
</style>

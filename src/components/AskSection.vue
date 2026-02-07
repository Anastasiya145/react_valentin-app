<script setup>
defineProps({
  stageRef: {
    type: Object,
    required: true,
  },
  noBtnRef: {
    type: Object,
    required: true,
  },
  noPos: {
    type: Object,
    required: true,
  },
  onYes: {
    type: Function,
    required: true,
  },
  onNo: {
    type: Function,
    required: true,
  },
  onMoveNo: {
    type: Function,
    required: true,
  },
});
</script>

<template>
  <section class="content">
    <div :ref="stageRef" class="button-stage">
      <button class="btn yes" type="button" @click="onYes">Oui, je veux ❤️</button>
      <button
        :ref="noBtnRef"
        class="btn no"
        type="button"
        :style="{ left: `${noPos.x}px`, top: `${noPos.y}px` }"
        @mouseenter="onMoveNo"
        @pointerenter="onMoveNo"
        @touchstart.prevent="onMoveNo"
        @click="onNo"
      >
        Non
      </button>
    </div>
    <p class="note">Attrape le bouton « Non » si tu peux 😏</p>
  </section>
</template>

<style scoped>
.content {
  margin-top: 28px;
}

.button-stage {
  position: relative;
  height: 180px;
  border-radius: 24px;
  background: linear-gradient(135deg, var(--color-stage-bg-1), var(--color-stage-bg-2));
  border: 1px dashed var(--color-stage-border);
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
}

.btn {
  border: none;
  font-family: var(--font-sans);
  font-weight: 700;
  border-radius: 999px;
  padding: 12px 26px;
  cursor: pointer;
  transition:
    transform 0.2s ease,
    box-shadow 0.2s ease;
}

.btn.yes {
  background: linear-gradient(135deg, var(--color-button-yes-1), var(--color-button-yes-2));
  color: white;
  box-shadow: 0 10px 24px var(--color-button-yes-shadow);
}

.btn.yes:hover {
  transform: translateY(-2px) scale(1.02);
}

.btn.no {
  position: absolute;
  background: #ffffff;
  color: var(--color-button-no-text);
  border: 2px solid var(--color-button-no-border);
  box-shadow: 0 8px 16px var(--color-button-no-shadow);
  transition:
    left 0.2s ease,
    top 0.2s ease;
}

.note {
  margin: 16px 0 0;
  font-size: 14px;
  color: var(--color-note);
  text-align: center;
}

@media (max-width: 520px) {
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
}
</style>

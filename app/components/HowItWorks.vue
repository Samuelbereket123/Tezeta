<template>
  <section class="how-it-works">
    <div class="container">
      <div class="section-header">
        <span class="badge">4 Simple Steps</span>
        <h2>From digital moments to <span class="accent">tactile books</span></h2>
        <p>Crafting your heirloom photo book is effortless. No complex design software—just your favorite memories bound beautifully.</p>
      </div>

      <div class="steps-grid">
        <div class="work-step" v-for="(step, idx) in steps" :key="idx">
          <div class="step-top">
            <span class="step-num">0{{ idx + 1 }}</span>
            <div class="step-icon-box">
              <component :is="step.icon" />
            </div>
          </div>
          <h3>{{ step.title }}</h3>
          <p>{{ step.description }}</p>
          <div class="step-highlight">
            <span>✨ {{ step.highlight }}</span>
          </div>
        </div>
      </div>

      <div class="cta-area">
        <button class="btn-primary btn-cta" @click="$emit('start')">
          <span>Start Creating Your Book</span>
          <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round" stroke-linejoin="round"><path d="M5 12h14"/><path d="m12 5 7 7-7 7"/></svg>
        </button>
        <button class="btn-text" @click="$emit('close')">Return to Home</button>
      </div>
    </div>
  </section>
</template>

<script setup>
import { h } from 'vue';

defineEmits(['start', 'close']);

const steps = [
  {
    title: 'Upload your collection',
    description: 'Select 25+ photos directly from your phone, computer, or cloud storage. Don’t worry about order yet.',
    highlight: 'Instant multi-photo drop & 1-click preset sample',
    icon: () => h('svg', { xmlns: 'http://www.w3.org/2000/svg', width: 26, height: 26, viewBox: '0 0 24 24', fill: 'none', stroke: 'currentColor', 'stroke-width': 1.8, 'stroke-linecap': 'round', 'stroke-linejoin': 'round' }, [
      h('path', { d: 'M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4' }),
      h('polyline', { points: '17 8 12 3 7 8' }),
      h('line', { x1: 12, y1: 3, x2: 12, y2: 15 })
    ])
  },
  {
    title: 'Design your cover',
    description: 'Choose between a full-bleed photo cover or custom color palette with timeless typography styles.',
    highlight: 'Custom titles & curated typography pairings',
    icon: () => h('svg', { xmlns: 'http://www.w3.org/2000/svg', width: 26, height: 26, viewBox: '0 0 24 24', fill: 'none', stroke: 'currentColor', 'stroke-width': 1.8, 'stroke-linecap': 'round', 'stroke-linejoin': 'round' }, [
      h('path', { d: 'M4 19.5V4a2 2 0 0 1 2-2h10.9a2 2 0 0 1 1.2.4l3.9 3.2a2 2 0 0 1 .1 3.3L16 15l-4-4-4 4-2-2z' }),
      h('path', { d: 'M8 22h12a2 2 0 0 0 2-2V7l-5-5H6a2 2 0 0 0-2 2v15.5a2.5 2.5 0 0 0 5 0V4' })
    ])
  },
  {
    title: 'Smart automatic layout',
    description: 'Our intelligent layout algorithm places your photos in balanced 2-page spreads. Drag & reorder anytime.',
    highlight: 'Full-bleed spread options & page navigation',
    icon: () => h('svg', { xmlns: 'http://www.w3.org/2000/svg', width: 26, height: 26, viewBox: '0 0 24 24', fill: 'none', stroke: 'currentColor', 'stroke-width': 1.8, 'stroke-linecap': 'round', 'stroke-linejoin': 'round' }, [
      h('rect', { x: 3, y: 3, width: 18, height: 18, rx: 2, ry: 2 }),
      h('line', { x1: 12, y1: 3, x2: 12, y2: 21 })
    ])
  },
  {
    title: 'Print & doorstep delivery',
    description: 'Printed on 140gsm textured paper with flat lay binding, packed carefully and shipped to your door.',
    highlight: 'Archival paper quality & satisfaction guarantee',
    icon: () => h('svg', { xmlns: 'http://www.w3.org/2000/svg', width: 26, height: 26, viewBox: '0 0 24 24', fill: 'none', stroke: 'currentColor', 'stroke-width': 1.8, 'stroke-linecap': 'round', 'stroke-linejoin': 'round' }, [
      h('path', { d: 'M6 2 3 6v14a2 2 0 0 0 2 2h14a2 2 0 0 0 2-2V6l-3-4Z' }),
      h('path', { d: 'M3 6h18' }),
      h('path', { d: 'M16 10a4 4 0 0 1-8 0' })
    ])
  }
];
</script>

<style scoped>
.how-it-works {
  padding: 90px 0 110px;
  background: white;
}

.section-header {
  text-align: center;
  margin-bottom: 70px;
  max-width: 680px;
  margin-left: auto;
  margin-right: auto;
}

.badge {
  display: inline-block;
  padding: 6px 18px;
  background: rgba(127, 176, 105, 0.12);
  color: var(--primary);
  border-radius: 50px;
  font-size: 0.85rem;
  font-weight: 700;
  letter-spacing: 0.5px;
  margin-bottom: 18px;
}

h2 {
  font-size: 3rem;
  font-weight: 700;
  margin-bottom: 16px;
  letter-spacing: -1px;
  color: var(--text-main);
}

.accent {
  color: var(--primary);
}

.section-header p {
  font-size: 1.15rem;
  color: var(--text-muted);
  line-height: 1.6;
}

.steps-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 32px;
  margin-bottom: 70px;
}

.work-step {
  padding: 36px;
  background: #fcfdfb;
  border-radius: var(--radius-lg);
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  border: 1px solid var(--border-color);
  display: flex;
  flex-direction: column;
  position: relative;
}

.work-step:hover {
  transform: translateY(-6px);
  border-color: var(--primary);
  box-shadow: 0 16px 36px rgba(127, 176, 105, 0.12);
  background: white;
}

.step-top {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 24px;
}

.step-num {
  font-family: 'Outfit', sans-serif;
  font-size: 2.2rem;
  font-weight: 800;
  color: rgba(127, 176, 105, 0.4);
}

.step-icon-box {
  width: 52px;
  height: 52px;
  background: white;
  border-radius: 14px;
  display: flex;
  align-items: center;
  justify-content: center;
  color: var(--primary);
  box-shadow: 0 6px 16px rgba(0,0,0,0.06);
  border: 1px solid rgba(0,0,0,0.04);
}

h3 {
  font-size: 1.4rem;
  font-weight: 700;
  margin-bottom: 10px;
  color: var(--text-main);
}

.work-step p {
  color: var(--text-muted);
  line-height: 1.6;
  font-size: 1rem;
  margin-bottom: 20px;
  flex: 1;
}

.step-highlight {
  background: rgba(127, 176, 105, 0.08);
  padding: 8px 14px;
  border-radius: 8px;
  display: inline-block;
  font-size: 0.82rem;
  font-weight: 600;
  color: var(--primary-hover);
}

.cta-area {
  text-align: center;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 16px;
}

.btn-cta {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 16px 36px;
  font-size: 1.1rem;
  border-radius: 50px;
}

.btn-text {
  background: none;
  border: none;
  color: var(--text-muted);
  font-weight: 600;
  font-size: 0.95rem;
  cursor: pointer;
}

.btn-text:hover {
  color: var(--primary);
}

@media (max-width: 860px) {
  .steps-grid {
    grid-template-columns: 1fr;
  }
}
</style>


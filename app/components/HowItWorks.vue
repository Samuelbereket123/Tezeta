<template>
  <section class="how-it-works">
    <div class="container">
      <div class="section-header">
        <span class="badge">Simple & Seamless</span>
        <h2>Your memories, beautifully bound.</h2>
        <p>We’ve simplified the process so you can focus on the stories, not the software.</p>
      </div>

      <div class="steps-grid">
        <div class="work-step">
          <div class="step-icon">
            <svg xmlns="http://www.w3.org/2000/svg" width="32" height="32" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"><path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4"/><polyline points="17 8 12 3 7 8"/><line x1="12" y1="3" x2="12" y2="15"/></svg>
          </div>
          <h3>1. Upload your collection</h3>
          <p>Choose at least 25 of your favorite digital photos. Don't worry about the order yet—just grab the moments that matter.</p>
        </div>

        <div class="work-step">
          <div class="step-icon">
            <svg xmlns="http://www.w3.org/2000/svg" width="32" height="32" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"><path d="M4 19.5V4a2 2 0 0 1 2-2h10.9a2 2 0 0 1 1.2.4l3.9 3.2a2 2 0 0 1 .1 3.3L16 15l-4-4-4 4-2-2z"/><path d="M8 22h12a2 2 0 0 0 2-2V7l-5-5H6a2 2 0 0 0-2 2v15.5a2.5 2.5 0 0 0 5 0V4"/></svg>
          </div>
          <h3>2. Design your cover</h3>
          <p>Craft a bold, poster-style cover. Pick a signature photo or a minimal color, then add your title in beautiful typography.</p>
        </div>

        <div class="work-step">
          <div class="step-icon">
            <svg xmlns="http://www.w3.org/2000/svg" width="32" height="32" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"><rect x="3" y="3" width="18" height="18" rx="2" ry="2"/><line x1="12" y1="3" x2="12" y2="21"/></svg>
          </div>
          <h3>3. Automatic layout</h3>
          <p>Our intelligent system arranges your photos into clean, spacious spreads. You can drag to reorder if you want a different flow.</p>
        </div>

        <div class="work-step">
          <div class="step-icon">
            <svg xmlns="http://www.w3.org/2000/svg" width="32" height="32" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"><path d="M6 2 3 6v14a2 2 0 0 0 2 2h14a2 2 0 0 0 2-2V6l-3-4Z"/><path d="M3 6h18"/><path d="M16 10a4 4 0 0 1-8 0"/></svg>
          </div>
          <h3>4. We print & deliver</h3>
          <p>Once you’re happy, we print your book on premium textured paper and ship it directly to your doorstep.</p>
        </div>
      </div>

      <div class="cta-area">
        <button class="btn-primary" @click="$emit('start')">Get started now</button>
        <button class="btn-text" @click="$emit('close')">Back to home</button>
      </div>
    </div>
  </section>
</template>

<script setup>
defineEmits(['start', 'close']);
</script>

<style scoped>
.how-it-works {
  padding: 80px 0;
  background: white;
}

.section-header {
  text-align: center;
  margin-bottom: 80px;
  max-width: 700px;
  margin-left: auto;
  margin-right: auto;
}

.badge {
  display: inline-block;
  padding: 6px 16px;
  background: var(--bg-color);
  color: var(--primary);
  border-radius: 50px;
  font-size: 0.85rem;
  font-weight: 600;
  margin-bottom: 24px;
}

h2 {
  font-size: 3rem;
  margin-bottom: 20px;
  letter-spacing: -1.5px;
}

.section-header p {
  font-size: 1.2rem;
  color: var(--text-muted);
}

.steps-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 40px;
  margin-bottom: 80px;
}

.work-step {
  padding: 40px;
  background: var(--bg-color);
  border-radius: var(--radius-lg);
  transition: var(--transition);
  border: 1px solid transparent;
}

.work-step:hover {
  transform: translateY(-5px);
  border-color: var(--primary);
  box-shadow: 0 10px 30px rgba(127, 176, 105, 0.1);
}

.step-icon {
  width: 60px;
  height: 60px;
  background: white;
  border-radius: 16px;
  display: flex;
  align-items: center;
  justify-content: center;
  color: var(--primary);
  margin-bottom: 24px;
  box-shadow: 0 4px 12px rgba(0,0,0,0.05);
}

h3 {
  font-size: 1.5rem;
  margin-bottom: 12px;
}

.work-step p {
  color: var(--text-muted);
  line-height: 1.6;
}

.cta-area {
  text-align: center;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 20px;
}

.btn-text {
  background: none;
  color: var(--text-muted);
  font-weight: 600;
}

.btn-text:hover {
  color: var(--primary);
}

@media (max-width: 768px) {
  .steps-grid {
    grid-template-columns: 1fr;
  }
}
</style>

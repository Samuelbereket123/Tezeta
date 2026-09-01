<template>
  <section class="cta">
    <div class="container">
      <div class="cta-card">
        <div class="cta-content">
          <h2>Ready to hold your <span class="accent">memories</span>?</h2>
          <p>Join over 12,000 creators and families who have preserved their stories with Tizeta-Pages.</p>
          <button class="btn-cta" @click="$emit('start')">
            <span>Create your first book</span>
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round">
              <path d="M5 12h14"></path>
              <path d="m12 5 7 7-7 7"></path>
            </svg>
          </button>
        </div>
        <div class="cta-decoration">
          <div class="circle c1"></div>
          <div class="circle c2"></div>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
defineEmits(['start']);
</script>

<style scoped>
.cta {
  padding: 80px 0 120px;
}

.cta-card {
  background: #1d1d1f;
  border-radius: var(--radius-lg);
  padding: 80px 32px;
  text-align: center;
  color: white;
  position: relative;
  overflow: hidden;
  box-shadow: var(--shadow-lg);
}

.cta-content {
  position: relative;
  z-index: 2;
  max-width: 620px;
  margin: 0 auto;
}

h2 {
  font-size: clamp(2.4rem, 4.2vw, 3.4rem);
  margin-bottom: 20px;
  color: white;
  letter-spacing: -0.035em;
  line-height: 1.12;
}

.accent {
  background: linear-gradient(135deg, #7fb069 0%, #a7c957 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}

p {
  font-size: 1.15rem;
  color: #a1a1a6;
  margin-bottom: 36px;
  line-height: 1.5;
  letter-spacing: -0.01em;
}

.btn-cta {
  background: #ffffff;
  color: #1d1d1f;
  padding: 16px 36px;
  font-size: 1.02rem;
  font-weight: 600;
  border-radius: var(--radius-full);
  display: inline-flex;
  align-items: center;
  gap: 10px;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.25);
  transition: var(--transition);
  letter-spacing: -0.01em;
}

.btn-cta svg {
  width: 18px;
  height: 18px;
  transition: transform 0.2s ease;
}

.btn-cta:hover {
  background: #f5f5f7;
  transform: translateY(-2px) scale(1.02);
  box-shadow: 0 8px 28px rgba(0, 0, 0, 0.35);
}

.btn-cta:hover svg {
  transform: translateX(4px);
}

.btn-cta:active {
  transform: translateY(0);
}

.cta-decoration .circle {
  position: absolute;
  border-radius: 50%;
  filter: blur(80px);
  pointer-events: none;
}

.c1 {
  width: 400px;
  height: 400px;
  background: rgba(82, 183, 136, 0.15);
  top: -150px;
  left: -100px;
}

.c2 {
  width: 350px;
  height: 350px;
  background: rgba(127, 176, 105, 0.12);
  bottom: -150px;
  right: -80px;
}

@media (max-width: 600px) {
  .cta-card {
    padding: 60px 20px;
  }
  .btn-cta {
    width: 100%;
    justify-content: center;
  }
}
</style>

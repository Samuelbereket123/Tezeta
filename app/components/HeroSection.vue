<template>
  <section class="hero">
    <div class="container hero-container">
      <!-- Apple-style subtle badge -->
      <div class="hero-badge">
        <span class="badge-dot"></span>
        <span class="badge-text">Effortless Memory Preservation</span>
      </div>

      <!-- Main Headline: 1 Upload 2 Design 3 Get your Book in the middle -->
      <h1 class="hero-headline">
        <div class="steps-flow">
          <div class="step-card">
            <span class="step-number">1</span>
            <span class="step-title">Upload</span>
          </div>

          <div class="step-separator" aria-hidden="true">
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round">
              <path d="M5 12h14"></path>
              <path d="m12 5 7 7-7 7"></path>
            </svg>
          </div>

          <div class="step-card">
            <span class="step-number">2</span>
            <span class="step-title">Design</span>
          </div>

          <div class="step-separator" aria-hidden="true">
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round">
              <path d="M5 12h14"></path>
              <path d="m12 5 7 7-7 7"></path>
            </svg>
          </div>

          <div class="step-card step-highlight">
            <span class="step-number highlight-num">3</span>
            <span class="step-title highlight-title">Get your Book</span>
          </div>
        </div>
      </h1>

      <!-- Clean Apple-style Subtitle -->
      <p class="hero-subtitle">
        Turn your favorite camera roll moments into an heirloom-quality physical book in minutes.
        No complicated tools—just your stories, beautifully bound.
      </p>

      <!-- Action Buttons -->
      <div class="hero-actions">
        <button class="btn-primary btn-hero-main" @click="$emit('start')">
          <span>Start creating your book</span>
          <svg class="btn-arrow" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round">
            <path d="M5 12h14"></path>
            <path d="m12 5 7 7-7 7"></path>
          </svg>
        </button>
        <button class="btn-secondary btn-hero-sub" @click="$emit('start')">
          <span>⚡ Try Quick Demo</span>
        </button>
      </div>

      <!-- Trust Features Minimalist Bar -->
      <div class="hero-trust-bar">
        <div class="trust-item">
          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round">
            <path d="M20 6L9 17l-5-5"/>
          </svg>
          <span>Hardcover & Layflat</span>
        </div>
        <span class="trust-dot">•</span>
        <div class="trust-item">
          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round">
            <path d="M20 6L9 17l-5-5"/>
          </svg>
          <span>Min. 25 Photos</span>
        </div>
        <span class="trust-dot">•</span>
        <div class="trust-item">
          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round">
            <path d="M20 6L9 17l-5-5"/>
          </svg>
          <span>Free Worldwide Shipping</span>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
defineEmits(['start']);
</script>

<style scoped>
.hero {
  padding: 100px 0 120px;
  position: relative;
  text-align: center;
}

.hero-container {
  max-width: 980px;
  margin: 0 auto;
  display: flex;
  flex-direction: column;
  align-items: center;
}

/* Badge */
.hero-badge {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  padding: 8px 18px;
  background: rgba(255, 255, 255, 0.85);
  backdrop-filter: blur(16px);
  -webkit-backdrop-filter: blur(16px);
  border: 1px solid rgba(0, 0, 0, 0.07);
  border-radius: var(--radius-full);
  font-size: 0.88rem;
  font-weight: 600;
  color: var(--text-main);
  margin-bottom: 32px;
  box-shadow: var(--shadow-sm);
  letter-spacing: -0.01em;
}

.badge-dot {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background-color: var(--primary);
  box-shadow: 0 0 0 3px rgba(45, 106, 79, 0.2);
  animation: pulseDot 2.2s infinite ease-in-out;
}

@keyframes pulseDot {
  0% { transform: scale(0.95); opacity: 0.8; }
  50% { transform: scale(1.2); opacity: 1; }
  100% { transform: scale(0.95); opacity: 0.8; }
}

/* Main Display Headline */
.hero-headline {
  margin: 0 0 24px;
  width: 100%;
}

.steps-flow {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  flex-wrap: wrap;
  gap: 16px 22px;
  padding: 14px 28px;
  background: rgba(255, 255, 255, 0.65);
  backdrop-filter: blur(20px);
  -webkit-backdrop-filter: blur(20px);
  border: 1px solid rgba(0, 0, 0, 0.06);
  border-radius: 28px;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.03);
}

.step-card {
  display: inline-flex;
  align-items: center;
  gap: 12px;
  font-size: clamp(2rem, 4.2vw, 3.2rem);
  font-weight: 800;
  letter-spacing: -0.04em;
  color: var(--text-main);
  line-height: 1.1;
  transition: transform 0.2s ease;
}

.step-card:hover {
  transform: translateY(-2px);
}

.step-number {
  width: 44px;
  height: 44px;
  border-radius: 50%;
  background: rgba(0, 0, 0, 0.05);
  color: var(--text-main);
  display: inline-flex;
  align-items: center;
  justify-content: center;
  font-size: 1.15rem;
  font-weight: 800;
  letter-spacing: normal;
  border: 1px solid rgba(0, 0, 0, 0.06);
}

.step-title {
  font-weight: 800;
}

.step-separator {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  color: #86868b;
  opacity: 0.6;
  width: 24px;
  height: 24px;
  animation: floatArrow 2.5s ease-in-out infinite;
}

.step-separator svg {
  width: 24px;
  height: 24px;
}

.step-separator:nth-of-type(2) {
  animation-delay: 0.4s;
}

@keyframes floatArrow {
  0%, 100% { transform: translateX(0); }
  50% { transform: translateX(4px); }
}

/* Highlight Step 3 */
.step-highlight .highlight-num {
  background: var(--primary);
  color: #ffffff;
  border-color: var(--primary);
  box-shadow: 0 4px 14px rgba(45, 106, 79, 0.35);
}

.step-highlight .highlight-title {
  background: linear-gradient(135deg, #1b4332 0%, var(--primary) 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}

/* Subtitle */
.hero-subtitle {
  font-size: clamp(1.15rem, 2.2vw, 1.35rem);
  color: var(--text-muted);
  max-width: 650px;
  margin: 16px auto 44px;
  line-height: 1.55;
  font-weight: 400;
  letter-spacing: -0.015em;
}

/* Actions */
.hero-actions {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 16px;
  margin-bottom: 56px;
}

.btn-hero-main {
  padding: 16px 36px;
  font-size: 1.05rem;
}

.btn-arrow {
  width: 18px;
  height: 18px;
  transition: transform 0.2s ease;
}

.btn-hero-main:hover .btn-arrow {
  transform: translateX(4px);
}

.btn-hero-sub {
  padding: 16px 32px;
  font-size: 1.05rem;
}

/* Trust Bar */
.hero-trust-bar {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 16px;
  font-size: 0.88rem;
  color: var(--text-light);
  font-weight: 500;
  letter-spacing: -0.01em;
}

.trust-item {
  display: flex;
  align-items: center;
  gap: 6px;
}

.trust-item svg {
  width: 15px;
  height: 15px;
  color: var(--primary);
}

.trust-dot {
  opacity: 0.4;
}

/* Responsive */
@media (max-width: 860px) {
  .hero {
    padding: 70px 0 80px;
  }
  .steps-flow {
    padding: 16px;
    gap: 12px 16px;
    border-radius: 20px;
  }
  .step-card {
    font-size: 1.8rem;
  }
  .step-number {
    width: 36px;
    height: 36px;
    font-size: 0.95rem;
  }
  .step-separator svg {
    width: 20px;
    height: 20px;
  }
}

@media (max-width: 600px) {
  .hero {
    padding: 50px 0 60px;
  }
  .steps-flow {
    flex-direction: column;
    width: 100%;
    padding: 20px 16px;
    gap: 14px;
    border-radius: 18px;
  }
  .step-card {
    font-size: 1.6rem;
    width: 100%;
    justify-content: center;
  }
  .step-separator {
    transform: rotate(90deg);
  }
  @keyframes floatArrow {
    0%, 100% { transform: rotate(90deg) translateY(0); }
    50% { transform: rotate(90deg) translateY(3px); }
  }
  .hero-actions {
    flex-direction: column;
    width: 100%;
    gap: 12px;
  }
  .btn-hero-main,
  .btn-hero-sub {
    width: 100%;
  }
  .hero-trust-bar {
    flex-direction: column;
    gap: 8px;
  }
  .trust-dot {
    display: none;
  }
}
</style>

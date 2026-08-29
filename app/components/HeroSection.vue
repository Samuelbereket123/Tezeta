<template>
  <section class="hero">
    <div class="container hero-content">
      <div class="hero-text">
        <span class="badge">
          <span class="pulse-dot"></span>
          Simple 3-Step Creation
        </span>
        <h1>From digital photos to <span class="accent">beautiful books</span>.</h1>
        <p>A calm, effortless way to turn your favorite moments into tactile heirloom pages. No design skills required.</p>
        
        <!-- Clean Step-by-Step Preview Strip -->
        <div class="hero-steps-preview">
          <div class="h-step-pill">
            <span class="h-step-num">1</span>
            <span>Upload 25+ Photos</span>
          </div>
          <div class="h-step-divider">→</div>
          <div class="h-step-pill">
            <span class="h-step-num">2</span>
            <span>Design Cover & Layout</span>
          </div>
          <div class="h-step-divider">→</div>
          <div class="h-step-pill">
            <span class="h-step-num">3</span>
            <span>Order & Print</span>
          </div>
        </div>

        <div class="hero-actions">
          <button class="btn-primary btn-hero-start" @click="$emit('start')">
            <span>Start Creating Now</span>
            <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M5 12h14"/><path d="m12 5 7 7-7 7"/></svg>
          </button>
          <button class="btn-secondary" @click="$emit('start')">
            ⚡ Quick Demo Flow
          </button>
        </div>
      </div>

      <div class="hero-image">
        <div class="book-mockup">
          <div class="book-cover">
            <div class="book-inner">
               <div class="book-spine"></div>
               <div class="book-front">
                 <img src="https://images.unsplash.com/photo-1544144433-d50aff500b91?q=80&w=2070&auto=format&fit=crop" alt="Book Cover" />
                 <div class="book-overlay"></div>
                 <div class="book-title">Summer Memories</div>
               </div>
            </div>
          </div>
          <div class="floating-photo p1">
            <img src="https://images.unsplash.com/photo-1502672260266-1c1ef2d93688?w=300&auto=format&fit=crop" alt="Photo 1" />
            <span class="photo-badge">Spread 01</span>
          </div>
          <div class="floating-photo p2">
            <img src="https://images.unsplash.com/photo-1542038784456-1ea8e935640e?w=300&auto=format&fit=crop" alt="Photo 2" />
            <span class="photo-badge">Cover Design</span>
          </div>
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
  padding: 70px 0 90px;
  overflow: hidden;
}

.hero-content {
  display: grid;
  grid-template-columns: 1.1fr 0.9fr;
  align-items: center;
  gap: 50px;
}

.badge {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  padding: 6px 16px;
  background: rgba(127, 176, 105, 0.12);
  color: #2E7D32;
  border-radius: 50px;
  font-size: 0.85rem;
  font-weight: 700;
  margin-bottom: 24px;
}

.pulse-dot {
  width: 8px;
  height: 8px;
  background: #2E7D32;
  border-radius: 50%;
  box-shadow: 0 0 0 0 rgba(46, 125, 50, 0.4);
  animation: pulse 1.8s infinite;
}

@keyframes pulse {
  0% { transform: scale(0.95); box-shadow: 0 0 0 0 rgba(46, 125, 50, 0.7); }
  70% { transform: scale(1); box-shadow: 0 0 0 8px rgba(46, 125, 50, 0); }
  100% { transform: scale(0.95); box-shadow: 0 0 0 0 rgba(46, 125, 50, 0); }
}

h1 {
  font-size: 3.8rem;
  line-height: 1.1;
  margin-bottom: 20px;
  letter-spacing: -2px;
  color: var(--text-main);
}

.accent {
  color: var(--primary);
}

p {
  font-size: 1.2rem;
  color: var(--text-muted);
  margin-bottom: 32px;
  max-width: 520px;
  line-height: 1.5;
}

.hero-steps-preview {
  display: flex;
  align-items: center;
  gap: 10px;
  background: white;
  padding: 10px 16px;
  border-radius: 50px;
  border: 1px solid var(--border-color);
  width: fit-content;
  margin-bottom: 36px;
  box-shadow: 0 4px 15px rgba(0,0,0,0.03);
}

.h-step-pill {
  display: flex;
  align-items: center;
  gap: 8px;
  font-size: 0.85rem;
  font-weight: 600;
  color: var(--text-main);
}

.h-step-num {
  width: 22px;
  height: 22px;
  background: rgba(127, 176, 105, 0.2);
  color: var(--primary-hover);
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 0.75rem;
  font-weight: 800;
}

.h-step-divider {
  color: var(--border-color);
  font-size: 0.85rem;
}

.hero-actions {
  display: flex;
  gap: 16px;
}

.btn-hero-start {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 14px 32px;
  font-size: 1.05rem;
}

.hero-image {
  position: relative;
  display: flex;
  justify-content: center;
}

.book-mockup {
  position: relative;
  z-index: 10;
}

.book-cover {
  width: 320px;
  height: 420px;
  perspective: 1000px;
}

.book-inner {
  width: 100%;
  height: 100%;
  position: relative;
  transform: rotateY(-18deg) rotateX(8deg);
  transform-style: preserve-3d;
  box-shadow: 25px 25px 60px rgba(0,0,0,0.15);
  border-radius: 4px 14px 14px 4px;
  overflow: hidden;
  transition: transform 0.5s ease;
}

.book-mockup:hover .book-inner {
  transform: rotateY(-8deg) rotateX(4deg) scale(1.02);
}

.book-front {
  position: absolute;
  width: 100%;
  height: 100%;
  background: white;
}

.book-front img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.book-overlay {
  position: absolute;
  inset: 0;
  background: linear-gradient(to top, rgba(0,0,0,0.5) 0%, transparent 60%);
}

.book-title {
  position: absolute;
  bottom: 30px;
  left: 24px;
  right: 24px;
  color: white;
  font-family: 'Outfit', sans-serif;
  font-weight: 700;
  font-size: 2.2rem;
  line-height: 1;
  text-shadow: 0 2px 10px rgba(0,0,0,0.4);
}

.book-spine {
  position: absolute;
  left: 0;
  width: 20px;
  height: 100%;
  background: #e0e0e0;
  transform: rotateY(-90deg);
  transform-origin: left;
}

.floating-photo {
  position: absolute;
  width: 130px;
  height: 160px;
  background: white;
  padding: 6px;
  box-shadow: 0 14px 35px rgba(0,0,0,0.12);
  border-radius: 8px;
  z-index: 5;
  transition: transform 0.3s ease;
  display: flex;
  flex-direction: column;
}

.floating-photo img {
  width: 100%;
  height: 120px;
  object-fit: cover;
  border-radius: 4px;
}

.photo-badge {
  font-size: 0.7rem;
  font-weight: 700;
  color: var(--text-muted);
  text-align: center;
  margin-top: 6px;
}

.p1 {
  top: -20px;
  right: -40px;
  transform: rotate(12deg);
}

.p2 {
  bottom: -10px;
  left: -60px;
  transform: rotate(-12deg);
}

.book-mockup:hover .p1 {
  transform: rotate(18deg) translateY(-8px);
}

.book-mockup:hover .p2 {
  transform: rotate(-18deg) translateY(8px);
}

@media (max-width: 968px) {
  .hero-content {
    grid-template-columns: 1fr;
    text-align: center;
    gap: 40px;
  }
  .hero-text p {
    margin-left: auto;
    margin-right: auto;
  }
  .hero-steps-preview {
    margin-left: auto;
    margin-right: auto;
    flex-wrap: wrap;
    justify-content: center;
  }
  .hero-actions {
    justify-content: center;
  }
  h1 {
    font-size: 3rem;
  }
}
</style>


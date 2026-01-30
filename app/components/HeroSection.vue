<template>
  <section class="hero">
    <div class="container hero-content">
      <div class="hero-text">
        <span class="badge">Preserve Your Memories</span>
        <h1>From digital photos to <span class="accent">beautiful books</span>.</h1>
        <p>A calm, simple way to turn your favorite moments into physical pages. No complex tools, just your stories.</p>
        <div class="hero-actions">
          <button class="btn-primary" @click="$emit('start')">Create your book</button>
          <button class="btn-secondary">View examples</button>
        </div>
      </div>
      <div class="hero-image">
        <div class="book-mockup">
          <div class="book-cover">
            <div class="book-inner">
               <div class="book-spine"></div>
               <div class="book-front">
                 <img src="https://images.unsplash.com/photo-1544144433-d50aff500b91?q=80&w=2070&auto=format&fit=crop" alt="Book Cover" />
                 <div class="book-title">Summer Memories</div>
               </div>
            </div>
          </div>
          <div class="floating-photo p1"></div>
          <div class="floating-photo p2"></div>
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
  padding: 80px 0;
  overflow: hidden;
}

.hero-content {
  display: grid;
  grid-template-columns: 1fr 1fr;
  align-items: center;
  gap: 60px;
}

.badge {
  display: inline-block;
  padding: 6px 16px;
  background: var(--secondary-green, #C8E6C9);
  color: #2E7D32;
  border-radius: 50px;
  font-size: 0.85rem;
  font-weight: 600;
  margin-bottom: 24px;
}

h1 {
  font-size: 4rem;
  line-height: 1.1;
  margin-bottom: 24px;
  letter-spacing: -2px;
}

.accent {
  color: var(--primary);
}

p {
  font-size: 1.25rem;
  color: var(--text-muted);
  margin-bottom: 40px;
  max-width: 500px;
}

.hero-actions {
  display: flex;
  gap: 16px;
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
  height: 400px;
  perspective: 1000px;
}

.book-inner {
  width: 100%;
  height: 100%;
  position: relative;
  transform: rotateY(-20deg) rotateX(10deg);
  transform-style: preserve-3d;
  box-shadow: 20px 20px 60px rgba(0,0,0,0.1);
  border-radius: 4px 12px 12px 4px;
  overflow: hidden;
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

.book-title {
  position: absolute;
  bottom: 40px;
  left: 30px;
  right: 30px;
  color: white;
  font-family: 'Outfit', sans-serif;
  font-weight: 700;
  font-size: 2.5rem;
  line-height: 1;
  text-shadow: 0 2px 10px rgba(0,0,0,0.3);
}

.book-spine {
  position: absolute;
  left: 0;
  width: 20px;
  height: 100%;
  background: #f0f0f0;
  transform: rotateY(-90deg);
  transform-origin: left;
}

.floating-photo {
  position: absolute;
  width: 120px;
  height: 150px;
  background: white;
  padding: 8px;
  box-shadow: 0 10px 30px rgba(0,0,0,0.1);
  border-radius: 4px;
  z-index: 5;
}

.p1 {
  top: -20px;
  right: -40px;
  transform: rotate(15deg);
  background-image: url('https://images.unsplash.com/photo-1502672260266-1c1ef2d93688?w=300&auto=format&fit=crop');
  background-size: cover;
}

.p2 {
  bottom: 0;
  left: -60px;
  transform: rotate(-10deg);
  background-image: url('https://images.unsplash.com/photo-1542038784456-1ea8e935640e?w=300&auto=format&fit=crop');
  background-size: cover;
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
  .hero-actions {
    justify-content: center;
  }
  h1 {
    font-size: 3rem;
  }
}
</style>

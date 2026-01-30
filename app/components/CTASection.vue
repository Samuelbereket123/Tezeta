<template>
  <section class="cta">
    <div class="container">
      <div class="cta-card">
        <div class="cta-content">
          <h2>Ready to hold your <span class="accent">memories</span>?</h2>
          <p>Join over 12,000 families who have preserved their stories with Tizeta-Pages.</p>
          <button class="btn-primary btn-large" @click="$emit('start')">Create your first book</button>
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
  padding: 100px 0 150px;
}

.cta-card {
  background: var(--text-main);
  border-radius: var(--radius-lg);
  padding: 80px 40px;
  text-align: center;
  color: white;
  position: relative;
  overflow: hidden;
}

.cta-content {
  position: relative;
  z-index: 2;
  max-width: 600px;
  margin: 0 auto;
}

h2 {
  font-size: 3.5rem;
  margin-bottom: 24px;
  color: white;
}

.accent {
  color: var(--primary);
}

p {
  font-size: 1.25rem;
  opacity: 0.8;
  margin-bottom: 40px;
}

.btn-large {
  padding: 16px 40px;
  font-size: 1.1rem;
}

.cta-decoration .circle {
  position: absolute;
  border-radius: 50%;
  background: var(--primary);
  opacity: 0.1;
  z-index: 1;
}

.c1 {
  width: 300px;
  height: 300px;
  top: -150px;
  left: -150px;
}

.c2 {
  width: 200px;
  height: 200px;
  bottom: -100px;
  right: -50px;
}

@media (max-width: 768px) {
  h2 {
    font-size: 2.5rem;
  }
}
</style>

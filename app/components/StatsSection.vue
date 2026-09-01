<template>
  <section class="stats">
    <div class="container">
      <div class="stats-grid">
        <div v-for="(stat, index) in stats" :key="index" class="stat-item">
          <div class="stat-value">{{ stat.value }}</div>
          <div class="stat-label">{{ stat.label }}</div>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
const stats = [
  { value: '12,400+', label: 'Happy Families' },
  { value: '45,000+', label: 'Books Printed' },
  { value: '1.2M+', label: 'Photos Preserved' },
  { value: '4.9 / 5', label: 'Customer Rating' }
];
</script>

<style scoped>
.stats {
  padding: 60px 0;
  background: rgba(255, 255, 255, 0.6);
  backdrop-filter: blur(20px);
  -webkit-backdrop-filter: blur(20px);
  border-top: 1px solid var(--border-color);
  border-bottom: 1px solid var(--border-color);
}

.stats-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 32px;
  text-align: center;
}

.stat-item {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.stat-value {
  font-family: var(--font-display);
  font-size: clamp(2.2rem, 3.8vw, 3rem);
  font-weight: 800;
  letter-spacing: -0.04em;
  color: var(--text-main);
  margin-bottom: 6px;
  line-height: 1.1;
}

.stat-label {
  font-size: 0.88rem;
  color: var(--text-muted);
  font-weight: 500;
  letter-spacing: -0.01em;
}

@media (max-width: 768px) {
  .stats-grid {
    grid-template-columns: repeat(2, 1fr);
    gap: 24px;
  }
}

@media (max-width: 480px) {
  .stats-grid {
    grid-template-columns: 1fr;
    gap: 20px;
  }
}
</style>

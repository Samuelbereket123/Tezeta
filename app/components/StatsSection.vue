<template>
  <section class="stats">
    <div class="container overflow-hidden">
      <div class="stats-grid">
        <div v-for="(stat, index) in stats" :key="index" class="stat-item" :style="{ transitionDelay: `${index * 100}ms` }">
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
  { value: '4.9/5', label: 'Average Rating' }
];
</script>

<style scoped>
.stats {
  padding: 80px 0;
  background: rgba(255, 255, 255, 0.4);
  backdrop-filter: blur(5px);
  border-top: 1px solid var(--border-color);
  border-bottom: 1px solid var(--border-color);
}

.stats-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 40px;
  text-align: center;
}

.stat-value {
  font-family: 'Outfit', sans-serif;
  font-size: 3rem;
  font-weight: 700;
  color: var(--primary);
  margin-bottom: 8px;
  line-height: 1;
}

.stat-label {
  font-size: 1rem;
  color: var(--text-muted);
  font-weight: 500;
  text-transform: uppercase;
  letter-spacing: 1px;
}

@media (max-width: 768px) {
  .stats-grid {
    grid-template-columns: repeat(2, 1fr);
    gap: 30px;
  }
  .stat-value {
    font-size: 2.25rem;
  }
}

@media (max-width: 480px) {
  .stats-grid {
    grid-template-columns: 1fr;
  }
}
</style>

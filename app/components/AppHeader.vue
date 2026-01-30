<template>
  <header class="header">
    <div class="container header-content">
      <div class="logo">
        <div class="logo-icon">T</div>
        <span class="logo-text">Tizeta-Pages</span>
      </div>
      <nav class="nav">
        <a href="#" @click.prevent="$emit('reset')" class="nav-link">Home</a>
        <a href="#" @click.prevent="$emit('show-how')" class="nav-link">How it works</a>
        <button class="btn-primary-sm" v-if="currentStep === 'hero'" @click="$emit('start')">Get Started</button>
      </nav>
    </div>
  </header>
</template>

<script setup>
defineProps(['currentStep']);
defineEmits(['start', 'reset']);
</script>

<style scoped>
.header {
  height: 80px;
  display: flex;
  align-items: center;
  position: sticky;
  top: 0;
  z-index: 100;
  background: rgba(252, 253, 251, 0.8);
  backdrop-filter: blur(10px);
}

.header-content {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 100%;
}

.logo {
  display: flex;
  align-items: center;
  gap: 12px;
}

.logo-icon {
  width: 36px;
  height: 36px;
  background: var(--primary);
  color: white;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 10px;
  font-family: 'Outfit', sans-serif;
  font-weight: 700;
  font-size: 1.2rem;
}

.logo-text {
  font-family: 'Outfit', sans-serif;
  font-weight: 600;
  font-size: 1.4rem;
  letter-spacing: -0.5px;
}

.nav {
  display: flex;
  align-items: center;
  gap: 32px;
}

.nav-link {
  text-decoration: none;
  color: var(--text-muted);
  font-weight: 500;
  font-size: 0.95rem;
  transition: var(--transition);
}

.nav-link:hover {
  color: var(--primary);
}

.btn-primary-sm {
  background-color: var(--primary);
  color: white;
  padding: 8px 20px;
  border-radius: var(--radius-md);
  font-size: 0.9rem;
  font-weight: 500;
}
</style>

<template>
  <header class="header">
    <div class="container header-content">
      <div class="logo" @click="$emit('reset')" style="cursor: pointer;">
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
defineEmits(['start', 'reset', 'show-how']);
</script>

<style scoped>
.header {
  height: 72px;
  display: flex;
  align-items: center;
  position: sticky;
  top: 0;
  z-index: 100;
  background: rgba(252, 253, 251, 0.82);
  backdrop-filter: blur(20px) saturate(180%);
  -webkit-backdrop-filter: blur(20px) saturate(180%);
  border-bottom: 1px solid rgba(0, 0, 0, 0.05);
  transition: var(--transition);
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
  gap: 10px;
  user-select: none;
}

.logo-icon {
  width: 34px;
  height: 34px;
  background: var(--text-main);
  color: white;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 9px;
  font-family: var(--font-display);
  font-weight: 800;
  font-size: 1.1rem;
  letter-spacing: -0.02em;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.12);
}

.logo-text {
  font-family: var(--font-display);
  font-weight: 700;
  font-size: 1.25rem;
  letter-spacing: -0.03em;
  color: var(--text-main);
}

.nav {
  display: flex;
  align-items: center;
  gap: 28px;
}

.nav-link {
  text-decoration: none;
  color: var(--text-muted);
  font-weight: 500;
  font-size: 0.92rem;
  transition: var(--transition);
  letter-spacing: -0.01em;
}

.nav-link:hover {
  color: var(--text-main);
}

.btn-primary-sm {
  background-color: var(--text-main);
  color: white;
  padding: 8px 18px;
  border-radius: var(--radius-full);
  font-size: 0.88rem;
  font-weight: 600;
  letter-spacing: -0.01em;
  box-shadow: var(--shadow-sm);
  transition: var(--transition);
}

.btn-primary-sm:hover {
  background-color: #000000;
  transform: translateY(-1px);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
}
</style>

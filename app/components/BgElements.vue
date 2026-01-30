<template>
  <div class="bg-elements">
    <div class="blob blob-1"></div>
    <div class="blob blob-2"></div>
    <div class="blob blob-3"></div>
    <div class="particles">
      <div v-for="i in 10" :key="i" class="particle" :style="particleStyle(i)"></div>
    </div>
    <div class="corner-pattern"></div>
    <div class="grid-overlay"></div>
    <div class="noise"></div>
  </div>
</template>

<script setup>
const particleStyle = (i) => {
  const left = Math.random() * 100;
  const duration = 20 + Math.random() * 30;
  const delay = Math.random() * -30;
  const size = 10 + Math.random() * 30;
  return {
    left: `${left}%`,
    top: '110%',
    width: `${size}px`,
    height: `${size}px`,
    animationDuration: `${duration}s`,
    animationDelay: `${delay}s`
  };
};
</script>

<style scoped>
/* ... existing styles ... */
.bg-elements {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: -1;
  overflow: hidden;
  pointer-events: none;
  background: var(--bg-color);
}

.blob {
  position: absolute;
  filter: blur(80px);
  border-radius: 50%;
  opacity: 0.5;
  animation: move 20s infinite alternate cubic-bezier(0.4, 0, 0.2, 1);
}

.blob-1 {
  width: 600px;
  height: 600px;
  background: radial-gradient(circle, #e8f5e9 0%, #a5d6a7 100%);
  top: -200px;
  right: -100px;
  animation-duration: 25s;
}

.blob-2 {
  width: 500px;
  height: 500px;
  background: radial-gradient(circle, #f1f8e9 0%, #c5e1a5 100%);
  bottom: -100px;
  left: -50px;
  animation-duration: 30s;
  animation-delay: -5s;
}

.blob-3 {
  width: 400px;
  height: 400px;
  background: radial-gradient(circle, #f9fbe7 0%, #e6ee9c 100%);
  top: 30%;
  left: 20%;
  animation-duration: 22s;
  animation-delay: -2s;
}

.particles {
  position: absolute;
  inset: 0;
}

.particle {
  position: absolute;
  width: 40px;
  height: 40px;
  border: 1px solid var(--primary);
  opacity: 0.1;
  border-radius: 8px;
  animation: float 20s infinite linear;
}

.particle:nth-child(even) {
  border-radius: 50%;
}

.corner-pattern {
  position: absolute;
  top: 0;
  right: 0;
  width: 300px;
  height: 300px;
  background-image: radial-gradient(var(--primary) 1px, transparent 1px);
  background-size: 20px 20px;
  opacity: 0.1;
  mask-image: linear-gradient(to bottom left, black, transparent);
  -webkit-mask-image: linear-gradient(to bottom left, black, transparent);
}

.grid-overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-image: 
    linear-gradient(var(--border-color) 1px, transparent 1px),
    linear-gradient(90deg, var(--border-color) 1px, transparent 1px);
  background-size: 50px 50px;
  opacity: 0.05;
}

.noise {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  opacity: 0.02;
  background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 200 200' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noiseFilter'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.65' numOctaves='3' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23noiseFilter)'/%3E%3C/svg%3E");
}

@keyframes move {
  0% { transform: translate(0, 0) scale(1) rotate(0deg); }
  50% { transform: translate(60px, 40px) scale(1.1) rotate(5deg); }
  100% { transform: translate(-20px, 80px) scale(1) rotate(0deg); }
}

@keyframes float {
  0% { transform: translateY(0) rotate(0deg); opacity: 0; }
  20% { opacity: 0.1; }
  80% { opacity: 0.1; }
  100% { transform: translateY(-120vh) rotate(360deg); opacity: 0; }
}
</style>

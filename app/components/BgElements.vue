<template>
  <div class="bg-elements" aria-hidden="true">
    <div class="glow-orb orb-1"></div>
    <div class="glow-orb orb-2"></div>
    <div class="glow-orb orb-3"></div>
    <div class="subtle-grid"></div>
  </div>
</template>

<style scoped>
.bg-elements {
  position: fixed;
  inset: 0;
  z-index: -1;
  overflow: hidden;
  pointer-events: none;
  background: var(--bg-color);
}

.glow-orb {
  position: absolute;
  filter: blur(120px);
  border-radius: 50%;
  opacity: 0.45;
  will-change: transform;
  animation: gentleFloat 24s infinite alternate ease-in-out;
}

.orb-1 {
  width: 650px;
  height: 650px;
  background: radial-gradient(circle, rgba(127, 176, 105, 0.25) 0%, rgba(82, 183, 136, 0.08) 60%, transparent 80%);
  top: -200px;
  left: 50%;
  transform: translateX(-50%);
}

.orb-2 {
  width: 500px;
  height: 500px;
  background: radial-gradient(circle, rgba(216, 243, 220, 0.5) 0%, rgba(183, 228, 199, 0.1) 70%, transparent 85%);
  bottom: 10%;
  left: -100px;
  animation-duration: 28s;
  animation-delay: -6s;
}

.orb-3 {
  width: 550px;
  height: 550px;
  background: radial-gradient(circle, rgba(235, 248, 235, 0.6) 0%, rgba(200, 230, 201, 0.1) 60%, transparent 80%);
  top: 40%;
  right: -150px;
  animation-duration: 32s;
  animation-delay: -12s;
}

.subtle-grid {
  position: absolute;
  inset: 0;
  background-image: 
    radial-gradient(rgba(0, 0, 0, 0.04) 1px, transparent 1px);
  background-size: 32px 32px;
  opacity: 0.6;
  mask-image: radial-gradient(ellipse 70% 60% at 50% 40%, black 20%, transparent 80%);
  -webkit-mask-image: radial-gradient(ellipse 70% 60% at 50% 40%, black 20%, transparent 80%);
}

@keyframes gentleFloat {
  0% { transform: translate(0, 0) scale(1); }
  50% { transform: translate(30px, 20px) scale(1.05); }
  100% { transform: translate(-25px, 35px) scale(0.98); }
}
</style>

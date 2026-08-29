<template>
  <div class="app-wrapper">
    <BgElements />
    <AppHeader :currentStep="currentStep" @start="startFlow" @reset="resetFlow" @show-how="showHowItWorks" />
    
    <main>
      <Transition name="fade" mode="out-in">
        <div v-if="currentStep === 'hero'">
          <HeroSection @start="startFlow" />
          <StatsSection />
          <StorySection />
          <TestimonialsSection />
          <CTASection @start="startFlow" />
        </div>
        
        <HowItWorks v-else-if="currentStep === 'how-it-works'" @start="startFlow" @close="resetFlow" />

        <div v-else :class="['container', { 'editor-container': currentStep === 'arrange' }]">
          <div class="stepper-progress">
            <div 
              v-for="(s, index) in steps" 
              :key="s.id" 
              class="step-item"
              :class="{ 
                'active': currentStep === s.id, 
                'completed': isCompleted(s.id) 
              }"
              @click="isCompleted(s.id) ? (currentStep = s.id) : null"
            >
              <div class="step-num">
                <svg v-if="isCompleted(s.id)" xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="3" stroke-linecap="round" stroke-linejoin="round"><polyline points="20 6 9 17 4 12"/></svg>
                <span v-else>{{ index + 1 }}</span>
              </div>
              <div class="step-label">{{ s.label }}</div>
            </div>
          </div>

          <UploadStep 
            v-if="currentStep === 'upload'" 
            v-model:photos="photos" 
            @next="nextStep" 
          />
          
          <ArrangeStep 
            v-if="currentStep === 'arrange'" 
            v-model:photos="photos"
            :cover="cover"
            @next="nextStep"
            @prev="prevStep"
          />

          <OrderStep 
            v-if="currentStep === 'order'" 
            :photos="photos"
            :cover="cover"
            @prev="prevStep"
            @complete="resetFlow"
          />
        </div>
      </Transition>
    </main>

    <footer class="main-footer" v-if="currentStep === 'hero' || currentStep === 'how-it-works'">
      <div class="container footer-content">
        <p>&copy; 2026 Tizeta-Pages. All rights reserved.</p>
        <div class="footer-links">
          <a href="#">Privacy</a>
          <a href="#">Terms</a>
          <a href="#">Contact</a>
        </div>
      </div>
    </footer>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';

const currentStep = ref('hero');
const photos = ref([]);
const cover = ref({
  title: '',
  type: 'photo',
  photo: '',
  color: '#7FB069',
  font: "'Outfit', sans-serif"
});

const steps = [
  { id: 'upload', label: 'Upload' },
  { id: 'arrange', label: 'Design' },
  { id: 'order', label: 'Order' }
];

const stepIds = steps.map(s => s.id);

const startFlow = () => {
  currentStep.value = 'upload';
};

const showHowItWorks = () => {
  currentStep.value = 'how-it-works';
};

const resetFlow = () => {
  currentStep.value = 'hero';
  photos.value = [];
  // Reset cover but keep some defaults
  cover.value = {
    title: '',
    type: 'photo',
    photo: '',
    color: '#7FB069',
    font: "'Outfit', sans-serif"
  };
};

const nextStep = () => {
  const currentIndex = stepIds.indexOf(currentStep.value);
  if (currentIndex < stepIds.length - 1) {
    currentStep.value = stepIds[currentIndex + 1];
    window.scrollTo({ top: 0, behavior: 'smooth' });
  }
};

const prevStep = () => {
  const currentIndex = stepIds.indexOf(currentStep.value);
  if (currentIndex > 0) {
    currentStep.value = stepIds[currentIndex - 1];
    window.scrollTo({ top: 0, behavior: 'smooth' });
  }
};

const isCompleted = (stepId) => {
  const currentIndex = stepIds.indexOf(currentStep.value);
  const targetIndex = stepIds.indexOf(stepId);
  return targetIndex < currentIndex;
};

const updateCover = (newCover) => {
  cover.value = newCover;
};

// SEO Meta
useHead({
  title: 'Tizeta-Pages | Beautiful Photo Books',
  meta: [
    { name: 'description', content: 'Create elegant, minimal photo books from your most precious memories.' }
  ]
});
</script>

<style scoped>
.app-wrapper {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

main {
  flex: 1;
}

.stepper-progress {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 40px;
  padding: 30px 0 20px;
  max-width: 540px;
  margin: 0 auto;
}

.step-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 8px;
  position: relative;
  flex: 1;
}

.step-item.completed {
  cursor: pointer;
}

.step-item:not(:last-child)::after {
  content: '';
  position: absolute;
  top: 19px;
  left: calc(50% + 22px);
  width: calc(100% - 4px);
  height: 2px;
  background: var(--border-color);
  z-index: 1;
  transition: background 0.3s ease;
}

.step-item.completed:not(:last-child)::after {
  background: var(--primary);
}

.step-num {
  width: 40px;
  height: 40px;
  border-radius: 50%;
  background: white;
  border: 2px solid var(--border-color);
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: 700;
  font-size: 0.95rem;
  color: var(--text-muted);
  position: relative;
  z-index: 2;
  transition: var(--transition);
  box-shadow: 0 2px 8px rgba(0,0,0,0.04);
}

.step-item.active .step-num {
  border-color: var(--primary);
  color: var(--primary);
  transform: scale(1.1);
  box-shadow: 0 0 0 4px rgba(127, 176, 105, 0.25);
  background: white;
}

.step-item.completed .step-num {
  background: var(--primary);
  border-color: var(--primary);
  color: white;
}

.step-label {
  font-size: 0.85rem;
  font-weight: 600;
  color: var(--text-muted);
  transition: var(--transition);
}

.step-item.active .step-label {
  color: var(--text-main);
  font-weight: 700;
}

.main-footer {
  padding: 60px 0;
  background: white;
  border-top: 1px solid var(--border-color);
}

.footer-content {
  display: flex;
  justify-content: space-between;
  align-items: center;
  color: var(--text-muted);
  font-size: 0.9rem;
}

.footer-links {
  display: flex;
  gap: 24px;
}

.footer-links a {
  color: inherit;
  text-decoration: none;
}

.footer-links a:hover {
  color: var(--primary);
}

@media (max-width: 768px) {
  .stepper-progress {
    gap: 10px;
  }
  .step-label {
    display: none;
  }
}
</style>

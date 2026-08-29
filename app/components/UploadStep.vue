<template>
  <div class="step-container">
    <div class="step-header">
      <span class="badge-step">Step 1 of 3</span>
      <h2>Upload your photos</h2>
      <p>Select at least 25 photos to create your custom memory book. We'll automatically curate your initial layout.</p>
    </div>

    <!-- Quick preset loader for hassle-free testing -->
    <div class="quick-demo-banner" v-if="photos.length < 25">
      <div class="demo-info">
        <span class="demo-icon">✨</span>
        <div>
          <strong>Want a fast preview?</strong>
          <span>Load 25 high-resolution sample photos in 1 click.</span>
        </div>
      </div>
      <button class="btn-demo" @click="loadSamplePhotos">
        ⚡ Load 25 Preset Photos
      </button>
    </div>

    <div class="template-selector-section">
      <h3>Choose a Template Style</h3>
      <div class="template-cards">
        <div 
          class="template-card" 
          :class="{ active: templateStyle === 'mix' }"
          @click="selectTemplate('mix')"
        >
          <div class="template-preview mix-preview">
            <div class="page-side"><div class="box full"></div></div>
            <div class="page-side"><div class="box split"></div><div class="box split"></div></div>
          </div>
          <h4>Storybook Mix</h4>
          <p>A balanced mix of dynamic layouts</p>
        </div>
        
        <div 
          class="template-card" 
          :class="{ active: templateStyle === 'single' }"
          @click="selectTemplate('single')"
        >
          <div class="template-preview single-preview">
            <div class="page-side"><div class="box full"></div></div>
            <div class="page-side"><div class="box full"></div></div>
          </div>
          <h4>Minimalist</h4>
          <p>Clean, one full photo per page</p>
        </div>
        
        <div 
          class="template-card" 
          :class="{ active: templateStyle === 'grid4' }"
          @click="selectTemplate('grid4')"
        >
          <div class="template-preview grid-preview">
            <div class="page-side grid-2x2">
              <div class="box"></div><div class="box"></div><div class="box"></div><div class="box"></div>
            </div>
            <div class="page-side grid-2x2">
              <div class="box"></div><div class="box"></div><div class="box"></div><div class="box"></div>
            </div>
          </div>
          <h4>Grid Collage</h4>
          <p>Max photos, 4 on every page</p>
        </div>
        
        <div 
          class="template-card" 
          :class="{ active: templateStyle === 'panoramic' }"
          @click="selectTemplate('panoramic')"
        >
          <div class="template-preview pan-preview">
            <div class="page-full"><div class="box pano"></div></div>
          </div>
          <h4>Panoramic</h4>
          <p>Big bold spreads across two pages</p>
        </div>
      </div>
    </div>

    <div 
      class="upload-area" 
      :class="{ 'is-dragging': isDragging, 'has-photos': photos.length > 0 }"
      @dragover.prevent="isDragging = true"
      @dragleave.prevent="isDragging = false"
      @drop.prevent="handleDrop"
      @click="$refs.fileInput.click()"
    >
      <input 
        type="file" 
        ref="fileInput" 
        multiple 
        accept="image/*" 
        style="display: none" 
        @change="handleFiles" 
      />
      
      <div class="upload-icon-wrapper">
        <svg class="upload-svg" xmlns="http://www.w3.org/2000/svg" width="40" height="40" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round">
          <path d="M4 14.899A7 7 0 1 1 15.71 8h1.79a4.5 4.5 0 0 1 2.5 8.242"/>
          <path d="M12 12v9"/>
          <path d="m16 16-4-4-4 4"/>
        </svg>
      </div>
      <h3>Click or drag photos here</h3>
      <p>Supports JPG, PNG & WEBP (Min. 25 photos recommended)</p>
    </div>

    <div v-if="photos.length > 0" class="preview-section">
      <div class="preview-header">
        <span class="preview-title">Selected Collection</span>
        <button class="btn-clear" @click="clearAllPhotos">Clear All</button>
      </div>
      <div class="photo-preview-grid">
        <div v-for="(photo, index) in photos" :key="index" class="photo-item">
          <img :src="photo.url" alt="Uploaded photo" />
          <span class="photo-number">{{ index + 1 }}</span>
          <button class="remove-photo" @click.stop="removePhoto(index)" title="Remove photo">&times;</button>
        </div>
      </div>
    </div>

    <div class="step-footer">
      <div class="progress-bar-wrapper">
        <div class="progress-info">
          <span>Upload Progress</span>
          <strong :class="{ 'ready': photos.length >= 25 }">{{ photos.length }} / 25 photos</strong>
        </div>
        <div class="progress-track">
          <div class="progress-fill" :style="{ width: `${Math.min(100, (photos.length / 25) * 100)}%` }"></div>
        </div>
      </div>

      <button 
        class="btn-primary btn-next" 
        :disabled="photos.length < 25"
        @click="$emit('next')"
      >
        <span>Continue to Layout & Design</span>
        <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M5 12h14"/><path d="m12 5 7 7-7 7"/></svg>
      </button>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const props = defineProps({
  photos: Array,
  templateStyle: {
    type: String,
    default: 'mix'
  }
});
const emit = defineEmits(['update:photos', 'update:templateStyle', 'next']);

const selectTemplate = (style) => {
  emit('update:templateStyle', style);
};

const isDragging = ref(false);

const sampleImages = [
  "https://images.unsplash.com/photo-1502672260266-1c1ef2d93688?w=600&auto=format&fit=crop",
  "https://images.unsplash.com/photo-1542038784456-1ea8e935640e?w=600&auto=format&fit=crop",
  "https://images.unsplash.com/photo-1516627145497-ae6968895b74?w=600&auto=format&fit=crop",
  "https://images.unsplash.com/photo-1544144433-d50aff500b91?w=600&auto=format&fit=crop",
  "https://images.unsplash.com/photo-1470071459604-3b5ec3a7fe05?w=600&auto=format&fit=crop",
  "https://images.unsplash.com/photo-1492691527719-9d1e07e534b4?w=600&auto=format&fit=crop",
  "https://images.unsplash.com/photo-1507525428034-b723cf961d3e?w=600&auto=format&fit=crop",
  "https://images.unsplash.com/photo-1519741497674-611481863552?w=600&auto=format&fit=crop",
  "https://images.unsplash.com/photo-1469854523086-cc02fe5d8800?w=600&auto=format&fit=crop",
  "https://images.unsplash.com/photo-1511895426328-dc8714191300?w=600&auto=format&fit=crop",
  "https://images.unsplash.com/photo-1476514525535-ce74f4581177?w=600&auto=format&fit=crop",
  "https://images.unsplash.com/photo-1530521954074-e64f6810b32d?w=600&auto=format&fit=crop",
  "https://images.unsplash.com/photo-1506744038136-46273834b3fb?w=600&auto=format&fit=crop",
  "https://images.unsplash.com/photo-1501785888041-af3ef285b470?w=600&auto=format&fit=crop",
  "https://images.unsplash.com/photo-1472214103451-9374bd1c798e?w=600&auto=format&fit=crop",
  "https://images.unsplash.com/photo-1447752875215-b2761acb3c5d?w=600&auto=format&fit=crop",
  "https://images.unsplash.com/photo-1433086966358-54859d0ed716?w=600&auto=format&fit=crop",
  "https://images.unsplash.com/photo-1500382017468-9049fed747ef?w=600&auto=format&fit=crop",
  "https://images.unsplash.com/photo-1426604966848-d7adac402bff?w=600&auto=format&fit=crop",
  "https://images.unsplash.com/photo-1470240731273-7821a6eeb6bd?w=600&auto=format&fit=crop",
  "https://images.unsplash.com/photo-1490730141103-6cac27aaab94?w=600&auto=format&fit=crop",
  "https://images.unsplash.com/photo-1500530855697-b586d89ba3ee?w=600&auto=format&fit=crop",
  "https://images.unsplash.com/photo-1533105079780-92b9be482077?w=600&auto=format&fit=crop",
  "https://images.unsplash.com/photo-1465146344425-f00d5f5c8f07?w=600&auto=format&fit=crop",
  "https://images.unsplash.com/photo-1518495973542-4542c06a5843?w=600&auto=format&fit=crop"
];

const loadSamplePhotos = () => {
  const sampleList = sampleImages.map((url, i) => ({
    file: null,
    url: url
  }));
  emit('update:photos', sampleList);
};

const clearAllPhotos = () => {
  emit('update:photos', []);
};

const handleFiles = (event) => {
  const files = Array.from(event.target.files);
  addFiles(files);
};

const handleDrop = (event) => {
  isDragging.value = false;
  const files = Array.from(event.dataTransfer.files).filter(f => f.type.startsWith('image/'));
  addFiles(files);
};

const addFiles = (files) => {
  const newPhotos = files.map(file => ({
    file,
    url: URL.createObjectURL(file)
  }));
  emit('update:photos', [...props.photos, ...newPhotos]);
};

const removePhoto = (index) => {
  const updated = [...props.photos];
  if (updated[index].file) {
    URL.revokeObjectURL(updated[index].url);
  }
  updated.splice(index, 1);
  emit('update:photos', updated);
};
</script>

<style scoped>
.step-container {
  max-width: 860px;
  margin: 0 auto;
  padding: 30px 20px 60px;
}

.step-header {
  text-align: center;
  margin-bottom: 32px;
}

.badge-step {
  display: inline-block;
  padding: 4px 12px;
  background: rgba(127, 176, 105, 0.15);
  color: var(--primary-hover);
  border-radius: 50px;
  font-size: 0.8rem;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 1px;
  margin-bottom: 12px;
}

.step-header h2 {
  font-size: 2.4rem;
  font-weight: 700;
  color: var(--text-main);
  margin-bottom: 8px;
}

.step-header p {
  color: var(--text-muted);
  font-size: 1.05rem;
  max-width: 580px;
  margin: 0 auto;
}

.quick-demo-banner {
  background: linear-gradient(135deg, #f0f7ef 0%, #e3f2e1 100%);
  border: 1px solid rgba(127, 176, 105, 0.3);
  border-radius: var(--radius-md);
  padding: 16px 24px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 28px;
  box-shadow: 0 4px 15px rgba(127, 176, 105, 0.08);
}

.demo-info {
  display: flex;
  align-items: center;
  gap: 12px;
}

.demo-icon {
  font-size: 1.5rem;
}

.demo-info strong {
  display: block;
  font-size: 0.95rem;
  color: var(--text-main);
}

.demo-info span {
  font-size: 0.85rem;
  color: var(--text-muted);
}

.btn-demo {
  background: var(--primary);
  color: white;
  padding: 10px 20px;
  border-radius: var(--radius-md);
  font-weight: 600;
  font-size: 0.9rem;
  transition: var(--transition);
  white-space: nowrap;
}

.btn-demo:hover {
  background: var(--primary-hover);
  transform: translateY(-1px);
  box-shadow: 0 4px 12px rgba(127, 176, 105, 0.3);
}

.template-selector-section {
  margin-bottom: 40px;
}

.template-selector-section h3 {
  font-size: 1.25rem;
  color: var(--text-main);
  margin-bottom: 16px;
  text-align: center;
}

.template-cards {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 16px;
}

.template-card {
  background: white;
  border: 2px solid var(--border-color);
  border-radius: var(--radius-md);
  padding: 16px;
  cursor: pointer;
  transition: all 0.2s ease;
  text-align: center;
}

.template-card:hover {
  border-color: rgba(127, 176, 105, 0.5);
  transform: translateY(-2px);
  box-shadow: 0 8px 24px rgba(0,0,0,0.06);
}

.template-card.active {
  border-color: var(--primary);
  background: rgba(127, 176, 105, 0.04);
  box-shadow: 0 0 0 2px var(--primary);
}

.template-preview {
  display: flex;
  background: #f1f5f9;
  border-radius: 4px;
  aspect-ratio: 1.6;
  margin-bottom: 12px;
  overflow: hidden;
  border: 1px solid #e2e8f0;
  box-shadow: inset 0 2px 4px rgba(0,0,0,0.02);
}

.page-side, .page-full {
  flex: 1;
  padding: 6px;
  display: flex;
  gap: 4px;
  flex-direction: column;
}

.page-side:first-child {
  border-right: 1px solid #e2e8f0;
}

.box {
  background: #cbd5e1;
  border-radius: 2px;
  width: 100%;
}

.box.full { flex: 1; }
.box.split { flex: 1; }
.box.pano { flex: 1; }

.grid-2x2 {
  display: grid;
  grid-template-columns: 1fr 1fr;
  grid-template-rows: 1fr 1fr;
  gap: 4px;
}
.grid-2x2 .box { height: 100%; }

.page-full {
  padding: 6px;
  display: flex;
}

.template-card h4 {
  font-size: 0.95rem;
  font-weight: 700;
  color: var(--text-main);
  margin-bottom: 4px;
}

.template-card p {
  font-size: 0.75rem;
  color: var(--text-muted);
  line-height: 1.3;
}

.upload-area {
  border: 2px dashed var(--border-color);
  border-radius: var(--radius-lg);
  padding: 50px 30px;
  text-align: center;
  transition: var(--transition);
  cursor: pointer;
  background: white;
  box-shadow: 0 4px 20px rgba(0,0,0,0.02);
}

.upload-area:hover, .upload-area.is-dragging {
  border-color: var(--primary);
  background: rgba(127, 176, 105, 0.04);
  transform: scale(1.005);
}

.upload-icon-wrapper {
  width: 70px;
  height: 70px;
  border-radius: 50%;
  background: rgba(127, 176, 105, 0.1);
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 0 auto 16px;
  color: var(--primary);
}

.upload-svg {
  transition: transform 0.3s ease;
}

.upload-area:hover .upload-svg {
  transform: translateY(-4px);
}

.upload-area h3 {
  font-size: 1.25rem;
  margin-bottom: 6px;
  color: var(--text-main);
}

.upload-area p {
  color: var(--text-muted);
  font-size: 0.9rem;
}

.preview-section {
  margin-top: 36px;
}

.preview-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 16px;
}

.preview-title {
  font-weight: 700;
  font-size: 1.05rem;
  color: var(--text-main);
}

.btn-clear {
  background: none;
  border: none;
  color: #e74c3c;
  font-weight: 600;
  font-size: 0.85rem;
  cursor: pointer;
}

.btn-clear:hover {
  text-decoration: underline;
}

.photo-preview-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(110px, 1fr));
  gap: 14px;
  max-height: 520px;
  overflow-y: auto;
  padding: 10px 6px;
  scrollbar-width: thin;
  scrollbar-color: var(--primary) transparent;
}

.photo-item {
  aspect-ratio: 1;
  position: relative;
  border-radius: var(--radius-sm);
  overflow: hidden;
  box-shadow: 0 4px 10px rgba(0,0,0,0.08);
  border: 1px solid var(--border-color);
  transition: transform 0.2s ease;
}

.photo-item:hover {
  transform: scale(1.04);
  z-index: 2;
}

.photo-item img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.photo-number {
  position: absolute;
  bottom: 4px;
  left: 4px;
  background: rgba(0,0,0,0.6);
  color: white;
  font-size: 0.7rem;
  padding: 2px 6px;
  border-radius: 4px;
  font-weight: 600;
}

.remove-photo {
  position: absolute;
  top: 4px;
  right: 4px;
  width: 22px;
  height: 22px;
  background: rgba(0,0,0,0.6);
  color: white;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 15px;
  border: none;
  cursor: pointer;
  transition: background 0.2s;
}

.remove-photo:hover {
  background: #e74c3c;
}

.step-footer {
  margin-top: 40px;
  background: white;
  border-radius: var(--radius-md);
  padding: 24px 30px;
  box-shadow: 0 4px 20px rgba(0,0,0,0.04);
  border: 1px solid var(--border-color);
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 30px;
}

.progress-bar-wrapper {
  flex: 1;
}

.progress-info {
  display: flex;
  justify-content: space-between;
  font-size: 0.9rem;
  margin-bottom: 8px;
  color: var(--text-muted);
}

.progress-info strong.ready {
  color: var(--primary);
}

.progress-track {
  height: 8px;
  background: #eef2ef;
  border-radius: 10px;
  overflow: hidden;
}

.progress-fill {
  height: 100%;
  background: linear-gradient(90deg, var(--primary) 0%, #a5d6a7 100%);
  transition: width 0.4s ease;
}

.btn-next {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 14px 28px;
  font-size: 1rem;
}

.btn-primary:disabled {
  background-color: #cbd5e1;
  color: #94a3b8;
  cursor: not-allowed;
  transform: none;
  box-shadow: none;
}

@media (max-width: 640px) {
  .quick-demo-banner {
    flex-direction: column;
    align-items: stretch;
    gap: 12px;
  }
  .step-footer {
    flex-direction: column;
    align-items: stretch;
  }
}
</style>


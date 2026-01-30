<template>
  <div class="step-container">
    <div class="step-header">
      <h2>Step 1: Upload your photos</h2>
      <p>Select at least 25 photos to create your book. We'll handle the layout for you.</p>
    </div>

    <div 
      class="upload-area" 
      :class="{ 'is-dragging': isDragging }"
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
      
      <div class="upload-icon">
        <svg xmlns="http://www.w3.org/2000/svg" width="48" height="48" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"><path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4"/><polyline points="17 8 12 3 7 8"/><line x1="12" y1="3" x2="12" y2="15"/></svg>
      </div>
      <h3>Click or drag photos here</h3>
      <p>Supports JPG, PNG (Min 25 photos recommended)</p>
    </div>

    <div v-if="photos.length > 0" class="photo-preview-grid">
      <div v-for="(photo, index) in photos" :key="index" class="photo-item">
        <img :src="photo.url" />
        <button class="remove-photo" @click.stop="removePhoto(index)">&times;</button>
      </div>
    </div>

    <div class="step-footer">
      <div class="photo-count" :class="{ 'warning': photos.length < 25 }">
        {{ photos.length }} / 25 photos selected
      </div>
      <button 
        class="btn-primary" 
        :disabled="photos.length < 25"
        @click="$emit('next')"
      >
        Continue to Cover Design
      </button>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const props = defineProps(['photos']);
const emit = defineEmits(['update:photos', 'next']);

const isDragging = ref(false);

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
  URL.revokeObjectURL(updated[index].url);
  updated.splice(index, 1);
  emit('update:photos', updated);
};
</script>

<style scoped>
.step-container {
  max-width: 800px;
  margin: 0 auto;
  padding: 40px 0;
}

.step-header {
  text-align: center;
  margin-bottom: 40px;
}

.step-header h2 {
  font-size: 2.5rem;
  margin-bottom: 8px;
}

.upload-area {
  border: 2px dashed var(--border-color);
  border-radius: var(--radius-lg);
  padding: 60px;
  text-align: center;
  transition: var(--transition);
  cursor: pointer;
  background: white;
}

.upload-area:hover, .upload-area.is-dragging {
  border-color: var(--primary);
  background: rgba(127, 176, 105, 0.05);
}

.upload-icon {
  color: var(--primary);
  margin-bottom: 20px;
}

.photo-preview-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(100px, 1fr));
  gap: 12px;
  margin-top: 40px;
}

.photo-item {
  aspect-ratio: 1;
  position: relative;
  border-radius: var(--radius-sm);
  overflow: hidden;
  box-shadow: 0 2px 8px rgba(0,0,0,0.1);
}

.photo-item img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.remove-photo {
  position: absolute;
  top: 4px;
  right: 4px;
  width: 20px;
  height: 20px;
  background: rgba(0,0,0,0.5);
  color: white;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 14px;
}

.step-footer {
  margin-top: 60px;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 20px;
}

.photo-count {
  font-weight: 600;
  color: var(--text-muted);
}

.photo-count.warning {
  color: #d63031;
}

.btn-primary:disabled {
  background-color: var(--border-color);
  cursor: not-allowed;
  transform: none;
  box-shadow: none;
}
</style>

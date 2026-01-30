<template>
  <div class="step-container">
    <div class="step-header">
      <h2>Step 2: Design your cover</h2>
      <p>Give your memories a title. Choose a photo or a solid color for a clean look.</p>
    </div>

    <div class="editor-layout">
      <!-- Preview -->
      <div class="preview-panel">
        <div 
          class="book-preview" 
          :style="{ 
            backgroundColor: cover.type === 'color' ? cover.color : '#fff',
            backgroundImage: cover.type === 'photo' && cover.photo ? `url(${cover.photo})` : 'none'
          }"
        >
          <div class="preview-overlay" v-if="cover.type === 'photo'"></div>
          <div 
            class="title-display" 
            :style="{ 
              color: cover.type === 'color' ? '#fff' : '#fff',
              fontFamily: cover.font 
            }"
          >
            {{ cover.title || 'Your Title Here' }}
          </div>
          <div class="author-display">Tizeta-Pages Collection</div>
        </div>
      </div>

      <!-- Controls -->
      <div class="controls-panel">
        <div class="control-group">
          <label>Title Text</label>
          <input 
            type="text" 
            v-model="cover.title" 
            placeholder="e.g. Summer in Italy" 
            maxlength="40"
          />
        </div>

        <div class="control-group">
          <label>Cover Type</label>
          <div class="toggle-group">
            <button 
              :class="{ active: cover.type === 'photo' }" 
              @click="cover.type = 'photo'"
            >Photo Cover</button>
            <button 
              :class="{ active: cover.type === 'color' }" 
              @click="cover.type = 'color'"
            >Plain Color</button>
          </div>
        </div>

        <div v-if="cover.type === 'photo'" class="control-group">
          <label>Select Photo</label>
          <div class="photo-selector">
            <div 
              v-for="(photo, index) in availablePhotos.slice(0, 6)" 
              :key="index"
              class="photo-thumb"
              :class="{ active: cover.photo === photo.url }"
              @click="cover.photo = photo.url"
            >
              <img :src="photo.url" />
            </div>
          </div>
        </div>

        <div v-else class="control-group">
          <label>Select Color</label>
          <div class="color-selector">
            <div 
              v-for="color in colors" 
              :key="color"
              class="color-thumb"
              :style="{ backgroundColor: color }"
              :class="{ active: cover.color === color }"
              @click="cover.color = color"
            ></div>
          </div>
        </div>

        <div class="control-group">
          <label>Font Style</label>
          <select v-model="cover.font">
            <option value="'Outfit', sans-serif">Modern Bold</option>
            <option value="'Playfair Display', serif">Classic Serif</option>
            <option value="'Inter', sans-serif">Minimalist</option>
            <option value="'Georgia', serif">Emotional</option>
          </select>
        </div>

        <div class="navigation-actions">
          <button class="btn-secondary" @click="$emit('prev')">Back</button>
          <button class="btn-primary" @click="$emit('next')">Arrange Photos</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { reactive, onMounted } from 'vue';

const props = defineProps(['availablePhotos', 'initialCover']);
const emit = defineEmits(['update:cover', 'next', 'prev']);

const cover = reactive({
  title: props.initialCover?.title || '',
  type: props.initialCover?.type || 'photo',
  photo: props.initialCover?.photo || props.availablePhotos[0]?.url,
  color: props.initialCover?.color || '#7FB069',
  font: props.initialCover?.font || "'Outfit', sans-serif"
});

const colors = ['#7FB069', '#2D3436', '#98C1D9', '#EE6C4D', '#3D5A80', '#55efc4'];

// Watch for changes and emit
import { watch } from 'vue';
watch(cover, (newVal) => {
  emit('update:cover', { ...newVal });
}, { deep: true });

onMounted(() => {
  if (!cover.photo && props.availablePhotos.length > 0) {
    cover.photo = props.availablePhotos[0].url;
  }
});
</script>

<style scoped>
.step-container {
  padding: 40px 0;
}

.step-header {
  text-align: center;
  margin-bottom: 40px;
}

.editor-layout {
  display: grid;
  grid-template-columns: 1fr 400px;
  gap: 60px;
  align-items: flex-start;
}

.preview-panel {
  display: flex;
  justify-content: center;
}

.book-preview {
  width: 400px;
  height: 500px;
  background-color: white;
  background-size: cover;
  background-position: center;
  border-radius: 4px 16px 16px 4px;
  display: flex;
  flex-direction: column;
  justify-content: flex-end;
  padding: 60px 40px;
  box-shadow: 20px 20px 60px rgba(0,0,0,0.1);
  position: relative;
  overflow: hidden;
  transition: var(--transition);
}

.preview-overlay {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: linear-gradient(to top, rgba(0,0,0,0.4) 0%, transparent 60%);
}

.title-display {
  position: relative;
  z-index: 10;
  font-size: 3.5rem;
  font-weight: 700;
  line-height: 1;
  word-break: break-word;
  margin-bottom: 20px;
  text-transform: uppercase;
}

.author-display {
  position: relative;
  z-index: 10;
  color: rgba(255,255,255,0.8);
  font-weight: 500;
  font-size: 0.9rem;
  text-transform: uppercase;
  letter-spacing: 2px;
}

.controls-panel {
  background: white;
  padding: 30px;
  border-radius: var(--radius-lg);
  border: 1px solid var(--border-color);
}

.control-group {
  margin-bottom: 24px;
}

.control-group label {
  display: block;
  font-weight: 600;
  margin-bottom: 8px;
  font-size: 0.9rem;
  color: var(--text-muted);
}

input[type="text"], select {
  width: 100%;
  padding: 12px;
  border: 1px solid var(--border-color);
  border-radius: var(--radius-sm);
  font-family: inherit;
  font-size: 1rem;
}

.toggle-group {
  display: flex;
  gap: 10px;
}

.toggle-group button {
  flex: 1;
  padding: 10px;
  background: #f8f9f8;
  border: 1px solid var(--border-color);
  border-radius: var(--radius-sm);
  font-weight: 500;
}

.toggle-group button.active {
  background: var(--primary);
  color: white;
  border-color: var(--primary);
}

.photo-selector, .color-selector {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 10px;
}

.photo-thumb, .color-thumb {
  aspect-ratio: 1;
  border-radius: 4px;
  cursor: pointer;
  border: 2px solid transparent;
  overflow: hidden;
}

.photo-thumb img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.photo-thumb.active, .color-thumb.active {
  border-color: var(--primary);
}

.navigation-actions {
  display: flex;
  gap: 12px;
  margin-top: 40px;
}

.navigation-actions > button {
  flex: 1;
}

@media (max-width: 968px) {
  .editor-layout {
    grid-template-columns: 1fr;
  }
}
</style>

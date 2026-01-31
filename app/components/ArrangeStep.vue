<template>
  <div class="editor-wrapper">
    <!-- Left Sidebar: Photo Library -->
    <aside class="photo-library">
      <div class="sidebar-header">
        <h3>Photos</h3>
        <span class="count">{{ photos.length }} uploaded</span>
      </div>
      <div class="photo-grid">
        <div 
          v-for="(photo, index) in photos" 
          :key="photo.url"
          class="photo-item"
          draggable="true"
          @dragstart="onPhotoDragStart($event, photo)"
          @dragend="onPhotoDragEnd"
        >
          <img :src="photo.url" />
          <div class="usage-count" v-if="getUsageCount(photo.url) > 0">
            {{ getUsageCount(photo.url) }}
          </div>
        </div>
      </div>
    </aside>

    <!-- Main Editor: Canvas -->
    <main class="editor-canvas">
      <div class="canvas-header">
        <div class="spread-title">
          {{ activeSpreadIndex === -1 ? 'Book Cover' : `Pages ${activeSpreadIndex * 2 + 1} – ${activeSpreadIndex * 2 + 2}` }}
        </div>
        <div class="actions">
          <button class="btn-secondary btn-sm" @click="$emit('prev')">Back</button>
          <button class="btn-primary btn-sm" @click="$emit('next')">Review & Order</button>
        </div>
      </div>

      <div class="canvas-body">
        <!-- Cover Editor View -->
        <div v-if="activeSpreadIndex === -1" class="cover-editor-view">
          <div 
            class="book-spread cover-spread"
            :class="{ 'dragging-over': isOverCover }"
            :style="getCoverStyle()"
            @dragover.prevent
            @dragenter="isOverCover = true"
            @dragleave="isOverCover = false"
            @drop="onDropOnCover"
          >
            <div class="spread-overlay no-pointer" v-if="cover.type === 'photo'"></div>
            <div class="cover-content" :class="{ 'no-pointer': isDragging }" :style="{ fontFamily: cover.font }">
              <input 
                type="text" 
                :value="cover.title" 
                class="cover-title-input" 
                placeholder="Enter Title..."
                @input="onCoverTitleInput"
              />
              <div class="cover-subtitle">Tizeta-Pages Collection</div>
            </div>
            <div class="drop-hint no-pointer" v-if="!cover.photo && cover.type === 'photo'">
              Drop a photo here
            </div>
          </div>
          
          <div class="cover-controls glass" :class="{ 'no-pointer': isDragging }">
            <div class="control-group">
              <label>Cover Type</label>
              <div class="toggle-group">
                <button :class="{ active: cover.type === 'photo' }" @click="$emit('update:cover', { ...cover, type: 'photo' })">Photo</button>
                <button :class="{ active: cover.type === 'color' }" @click="$emit('update:cover', { ...cover, type: 'color' })">Color</button>
              </div>
            </div>
            <div class="control-group" v-if="cover.type === 'color'">
              <label>Background Color</label>
              <div class="color-picker">
                <div 
                  v-for="c in ['#7FB069', '#2D3436', '#3D5A80', '#EE6C4D', '#98C1D9']" 
                  :key="c"
                  class="color-option"
                  :style="{ backgroundColor: c }"
                  :class="{ active: cover.color === c }"
                  @click="$emit('update:cover', { ...cover, color: c })"
                ></div>
              </div>
            </div>
            <div class="control-group">
              <label>Font Style</label>
              <select :value="cover.font" @change="e => $emit('update:cover', { ...cover, font: e.target.value })">
                <option value="'Outfit', sans-serif">Modern Bold</option>
                <option value="'Playfair Display', serif">Classic Serif</option>
                <option value="'Inter', sans-serif">Minimalist</option>
              </select>
            </div>
          </div>
        </div>

        <!-- Pages Editor View -->
        <div v-else class="pages-editor-view">
          <div 
            class="book-spread" 
            :class="{ 'full-spread-active': bookLayout[activeSpreadIndex].isFull }"
            @dragover.prevent
          >
            <!-- Single Pages Mode -->
            <template v-if="!bookLayout[activeSpreadIndex].isFull">
              <div 
                class="page left-page" 
                :class="{ 
                  'empty': !bookLayout[activeSpreadIndex].left,
                  'dragging-over': draggingOverTarget === 'left'
                }"
                @dragover.prevent
                @dragenter="draggingOverTarget = 'left'"
                @dragleave="draggingOverTarget = null"
                @drop="onDropOnPage(activeSpreadIndex, 'left')"
              >
                <img v-if="bookLayout[activeSpreadIndex].left" :src="bookLayout[activeSpreadIndex].left.url" class="no-pointer" />
                <div v-else class="drop-label no-pointer">Drop photo (Left)</div>
                <button v-if="bookLayout[activeSpreadIndex].left" class="remove-photo" @click="clearPhoto(activeSpreadIndex, 'left')">×</button>
              </div>
              <div 
                class="page right-page" 
                :class="{ 
                  'empty': !bookLayout[activeSpreadIndex].right,
                  'dragging-over': draggingOverTarget === 'right'
                }"
                @dragover.prevent
                @dragenter="draggingOverTarget = 'right'"
                @dragleave="draggingOverTarget = null"
                @drop="onDropOnPage(activeSpreadIndex, 'right')"
              >
                <img v-if="bookLayout[activeSpreadIndex].right" :src="bookLayout[activeSpreadIndex].right.url" class="no-pointer" />
                <div v-else class="drop-label no-pointer">Drop photo (Right)</div>
                <button v-if="bookLayout[activeSpreadIndex].right" class="remove-photo" @click="clearPhoto(activeSpreadIndex, 'right')">×</button>
              </div>
            </template>

            <!-- Full Spread Mode -->
            <div 
              v-else 
              class="full-spread-container"
              :class="{ 'dragging-over': draggingOverTarget === 'full' }"
              @dragover.prevent
              @dragenter="draggingOverTarget = 'full'"
              @dragleave="draggingOverTarget = null"
              @drop="onDropOnPage(activeSpreadIndex, 'full')"
            >
              <img v-if="bookLayout[activeSpreadIndex].full" :src="bookLayout[activeSpreadIndex].full.url" class="no-pointer" />
              <div v-else class="drop-label no-pointer">Drop big photo for both pages</div>
              <button v-if="bookLayout[activeSpreadIndex].full" class="remove-photo" @click="clearPhoto(activeSpreadIndex, 'full')">×</button>
            </div>

            <!-- Global Spread Dropzone for "Make Full" -->
            <div 
              class="full-spread-overlay" 
              @dragover.prevent
              @drop="onDropOnSpread(activeSpreadIndex)"
              v-if="!bookLayout[activeSpreadIndex].isFull"
            >
              <span>Drop here for Full Spread</span>
            </div>
          </div>
          
          <div class="spread-tools">
            <button 
              class="btn-secondary btn-sm" 
              :class="{ active: bookLayout[activeSpreadIndex].isFull }"
              @click="toggleFullSpread(activeSpreadIndex)"
            >
              {{ bookLayout[activeSpreadIndex].isFull ? 'Switch to 2 Pages' : 'Make Full Spread' }}
            </button>
            <p class="tool-hint" v-if="!bookLayout[activeSpreadIndex].isFull">
              Tip: Drag a photo to the center line to make it a full spread.
            </p>
          </div>
        </div>
      </div>
    </main>

    <!-- Right Sidebar: Page Navigator -->
    <aside class="page-navigator">
      <div class="sidebar-header">
        <h3>Navigator</h3>
      </div>
      <div class="nav-list">
        <!-- Cover Item -->
        <div 
          class="nav-item cover-item" 
          :class="{ active: activeSpreadIndex === -1 }"
          @click="activeSpreadIndex = -1"
        >
          <div class="nav-preview cover-preview" :style="getCoverStyle()"></div>
          <div class="nav-label">Cover</div>
        </div>

        <!-- Spreads -->
        <div 
          v-for="(spread, index) in bookLayout" 
          :key="index"
          class="nav-item"
          :class="{ active: activeSpreadIndex === index }"
          @click="activeSpreadIndex = index"
        >
          <div class="nav-preview spread-preview">
            <template v-if="!spread.isFull">
              <div class="p-left"><img v-if="spread.left" :src="spread.left.url" /></div>
              <div class="p-right"><img v-if="spread.right" :src="spread.right.url" /></div>
            </template>
            <div v-else class="p-full"><img v-if="spread.full" :src="spread.full.url" /></div>
          </div>
          <div class="nav-label">Pages {{ index * 2 + 1 }}-{{ index * 2 + 2 }}</div>
        </div>
        
        <button class="add-spread-btn" @click="addSpread">+ Add Pages</button>
      </div>
    </aside>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';

const props = defineProps(['photos', 'cover']);
const emit = defineEmits(['update:photos', 'update:cover', 'next', 'prev']);

const activeSpreadIndex = ref(-1); // -1 is Cover
const draggedPhoto = ref(null);
const isDragging = ref(false);
const isOverCover = ref(false);
const draggingOverTarget = ref(null);

// Initialize Book Layout from props.photos
const bookLayout = ref([]);

const initLayout = () => {
  const spreads = [];
  for (let i = 0; i < props.photos.length; i += 2) {
    spreads.push({
      left: props.photos[i] || null,
      right: props.photos[i + 1] || null,
      isFull: false,
      full: null
    });
  }
  // Ensure minimum pages (e.g. 12 spreads = 24 pages + cover)
  while (spreads.length < 12) {
    spreads.push({ left: null, right: null, isFull: false, full: null });
  }
  bookLayout.value = spreads;
};

onMounted(() => {
  initLayout();
});

const onPhotoDragStart = (event, photo) => {
  draggedPhoto.value = photo;
  event.dataTransfer.effectAllowed = 'copy';
  isDragging.value = true;
};

const onPhotoDragEnd = () => {
  isDragging.value = false;
  isOverCover.value = false;
  draggingOverTarget.value = null;
};

const onDropOnCover = () => {
  if (draggedPhoto.value) {
    emit('update:cover', { ...props.cover, photo: draggedPhoto.value.url, type: 'photo' });
    onPhotoDragEnd();
  }
};

const onDropOnPage = (index, side) => {
  if (!draggedPhoto.value) return;
  
  if (side === 'left') {
    bookLayout.value[index].left = draggedPhoto.value;
    bookLayout.value[index].isFull = false;
  } else if (side === 'right') {
    bookLayout.value[index].right = draggedPhoto.value;
    bookLayout.value[index].isFull = false;
  } else if (side === 'full') {
    bookLayout.value[index].full = draggedPhoto.value;
    bookLayout.value[index].isFull = true;
  }
  syncPhotos();
  draggedPhoto.value = null;
};

const onDropOnSpread = (index) => {
  if (!draggedPhoto.value) return;
  bookLayout.value[index].isFull = true;
  bookLayout.value[index].full = draggedPhoto.value;
  syncPhotos();
  draggedPhoto.value = null;
};

const clearPhoto = (index, side) => {
  if (side === 'left') bookLayout.value[index].left = null;
  if (side === 'right') bookLayout.value[index].right = null;
  if (side === 'full') bookLayout.value[index].full = null;
  syncPhotos();
};

const toggleFullSpread = (index) => {
  bookLayout.value[index].isFull = !bookLayout.value[index].isFull;
  syncPhotos();
};

const addSpread = () => {
  bookLayout.value.push({ left: null, right: null, isFull: false, full: null });
};

const syncPhotos = () => {
  const usedPhotos = [];
  bookLayout.value.forEach(spread => {
    if (spread.isFull) {
      if (spread.full) usedPhotos.push(spread.full);
    } else {
      if (spread.left) usedPhotos.push(spread.left);
      if (spread.right) usedPhotos.push(spread.right);
    }
  });
  emit('update:photos', usedPhotos);
};

const onCoverTitleInput = (e) => {
  emit('update:cover', { ...props.cover, title: e.target.value });
};

const updateCover = () => {
  emit('update:cover', { ...props.cover });
};

const getCoverStyle = () => {
  if (props.cover.type === 'color') {
    return { backgroundColor: props.cover.color };
  }
  return { 
    backgroundImage: props.cover.photo ? `url(${props.cover.photo})` : 'none',
    backgroundSize: 'cover',
    backgroundPosition: 'center',
    backgroundColor: '#eee'
  };
};

const getUsageCount = (url) => {
  let count = 0;
  if (props.cover.photo === url) count++;
  bookLayout.value.forEach(s => {
    if (s.isFull && s.full?.url === url) count++;
    else {
      if (s.left?.url === url) count++;
      if (s.right?.url === url) count++;
    }
  });
  return count;
};
</script>

<style scoped>
.editor-wrapper {
  display: grid;
  grid-template-columns: 280px 1fr 240px;
  height: calc(100vh - 180px);
  min-height: 600px;
  background: white;
  border-radius: var(--radius-lg);
  overflow: hidden;
  box-shadow: var(--shadow);
  border: 1px solid var(--border-color);
}

.no-pointer {
  pointer-events: none;
}

.dragging-over {
  box-shadow: 0 0 0 4px var(--primary) !important;
  transform: scale(1.02);
}

aside {
  background: #fcfdfb;
  display: flex;
  flex-direction: column;
  overflow: hidden;
  border-right: 1px solid var(--border-color);
}

.page-navigator {
  border-right: none;
  border-left: 1px solid var(--border-color);
}

.sidebar-header {
  padding: 20px;
  border-bottom: 1px solid var(--border-color);
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.sidebar-header h3 {
  font-size: 0.9rem;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 1px;
  margin: 0;
}

.count {
  font-size: 0.75rem;
  color: var(--text-muted);
}

.photo-grid {
  flex: 1;
  overflow-y: auto;
  padding: 15px;
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 12px;
  align-content: flex-start;
}

.photo-item {
  aspect-ratio: 1;
  border-radius: 6px;
  overflow: hidden;
  cursor: grab;
  position: relative;
  border: 1px solid var(--border-color);
  background: #eee;
}

.photo-item:active {
  cursor: grabbing;
}

.photo-item img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.usage-count {
  position: absolute;
  top: 4px;
  right: 4px;
  background: var(--primary);
  color: white;
  width: 18px;
  height: 18px;
  border-radius: 50%;
  font-size: 0.7rem;
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: 700;
}

.editor-canvas {
  display: flex;
  flex-direction: column;
  background: #f0f2f0;
  position: relative;
}

.canvas-header {
  padding: 15px 30px;
  background: white;
  border-bottom: 1px solid var(--border-color);
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.spread-title {
  font-weight: 600;
  color: var(--text-main);
  font-family: 'Outfit', sans-serif;
}

.actions {
  display: flex;
  gap: 12px;
}

.canvas-body {
  flex: 1;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 40px;
  overflow-y: auto;
}

.book-spread {
  width: 100%;
  max-width: 800px;
  aspect-ratio: 1.6;
  background: white;
  display: flex;
  box-shadow: 0 30px 60px rgba(0,0,0,0.12);
  position: relative;
  transition: all 0.3s ease;
  border-radius: 4px;
}

.page {
  flex: 1;
  border: 1px solid #f0f0f0;
  position: relative;
  background: white;
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
}

.left-page {
  border-radius: 4px 0 0 4px;
  border-right: 1px solid #eee;
}

.right-page {
  border-radius: 0 4px 4px 0;
}

.page img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.empty {
  background: #f8f9f8;
}

.drop-label {
  font-size: 0.8rem;
  color: var(--text-muted);
  border: 2px dashed #ddd;
  padding: 15px;
  border-radius: 8px;
  text-align: center;
  pointer-events: none;
}

.full-spread-container {
  width: 100%;
  height: 100%;
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
}

.full-spread-container img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.full-spread-overlay {
  position: absolute;
  top: 0;
  left: 45%;
  width: 10%;
  height: 100%;
  background: rgba(127, 176, 105, 0.1);
  border-left: 2px dashed var(--primary);
  border-right: 2px dashed var(--primary);
  display: flex;
  align-items: center;
  justify-content: center;
  text-align: center;
  font-size: 0.65rem;
  color: var(--primary);
  font-weight: 700;
  text-transform: uppercase;
  opacity: 0;
  transition: opacity 0.2s;
  z-index: 10;
  pointer-events: all;
}

.book-spread:hover .full-spread-overlay {
  opacity: 1;
}

.cover-spread {
  max-width: 440px;
  aspect-ratio: 0.8;
  border-radius: 4px 12px 12px 4px;
  display: flex;
  flex-direction: column;
  justify-content: flex-end;
  padding: 60px 40px;
}

.spread-overlay {
  position: absolute;
  inset: 0;
  background: linear-gradient(to top, rgba(0,0,0,0.6) 0%, transparent 60%);
}

.cover-content {
  position: relative;
  z-index: 2;
}

.cover-title-input {
  background: transparent;
  border: none;
  border-bottom: 2px solid rgba(255,255,255,0.2);
  color: white;
  font-size: 2.8rem;
  font-weight: 700;
  width: 100%;
  margin-bottom: 15px;
  text-transform: uppercase;
  outline: none;
  font-family: inherit;
}

.cover-subtitle {
  color: rgba(255,255,255,0.8);
  font-size: 0.85rem;
  text-transform: uppercase;
  letter-spacing: 2px;
  font-weight: 500;
}

.cover-controls {
  position: absolute;
  bottom: 0px;
  left: 50%;
  transform: translateX(-50%) translateY(120%);
  padding: 20px;
  border-radius: var(--radius-lg);
  display: flex;
  gap: 30px;
  width: auto;
  min-width: 500px;
  justify-content: center;
  background: rgba(255, 255, 255, 0.9);
  backdrop-filter: blur(10px);
  box-shadow: 0 10px 30px rgba(0,0,0,0.1);
}

.remove-photo {
  position: absolute;
  top: 15px;
  right: 15px;
  width: 28px;
  height: 28px;
  background: rgba(0,0,0,0.4);
  color: white;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 1.4rem;
  border: none;
  cursor: pointer;
  z-index: 5;
}

.nav-list {
  flex: 1;
  overflow-y: auto;
  padding: 20px;
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.nav-item {
  cursor: pointer;
  transition: transform 0.2s;
}

.nav-item:hover {
  transform: scale(1.02);
}

.nav-preview {
  aspect-ratio: 1.6;
  background: white;
  border: 1px solid var(--border-color);
  border-radius: 6px;
  margin-bottom: 8px;
  display: flex;
  overflow: hidden;
  box-shadow: 0 2px 8px rgba(0,0,0,0.05);
}

.nav-item.active .nav-preview {
  border-color: var(--primary);
  box-shadow: 0 0 0 2px rgba(127, 176, 105, 0.2);
}

.nav-label {
  font-size: 0.7rem;
  text-align: center;
  color: var(--text-muted);
  font-weight: 600;
}

.cover-preview {
  aspect-ratio: 0.8;
  width: 70%;
  margin: 0 auto 8px;
}

.p-left { border-right: 1px solid #f0f0f0; flex: 1; }
.p-right { flex: 1; }
.p-full { width: 100%; }
.nav-preview img { width: 100%; height: 100%; object-fit: cover; }

.add-spread-btn {
  width: 100%;
  padding: 12px;
  border: 2px dashed var(--border-color);
  background: transparent;
  color: var(--text-muted);
  border-radius: 8px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.2s;
}

.add-spread-btn:hover {
  border-color: var(--primary);
  color: var(--primary);
  background: #fcfdfb;
}

.spread-tools {
  margin-top: 40px;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 12px;
}

.btn-sm { padding: 10px 20px; font-size: 0.85rem; font-weight: 600; }

.toggle-group {
  display: flex;
  gap: 4px;
  background: #eee;
  padding: 4px;
  border-radius: 8px;
}

.toggle-group button {
  padding: 6px 16px;
  font-size: 0.8rem;
  border-radius: 6px;
  border: none;
  cursor: pointer;
}

.toggle-group button.active {
  background: white;
  box-shadow: 0 2px 4px rgba(0,0,0,0.05);
  color: var(--primary);
  font-weight: 700;
}

.color-picker { display: flex; gap: 10px; }
.color-option {
  width: 24px;
  height: 24px;
  border-radius: 50%;
  cursor: pointer;
  border: 2px solid transparent;
  transition: transform 0.2s;
}
.color-option:hover { transform: scale(1.1); }
.color-option.active { border-color: white; box-shadow: 0 0 0 2px var(--primary); }

label {
  display: block;
  font-size: 0.7rem;
  font-weight: 700;
  text-transform: uppercase;
  margin-bottom: 8px;
  color: var(--text-muted);
  letter-spacing: 0.5px;
}

@media (max-width: 1400px) {
  .editor-wrapper { margin: 0 -40px; }
}

@media (max-width: 1200px) {
  .editor-wrapper {
    margin: 0;
    grid-template-columns: 200px 1fr 160px;
  }
}
</style>

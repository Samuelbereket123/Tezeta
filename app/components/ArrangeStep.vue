<template>
  <div class="editor-wrapper">
    <!-- Left Sidebar: Photo Library -->
    <aside class="photo-library">
      <div class="sidebar-header">
        <div>
          <h3>Photo Library</h3>
          <span class="count">{{ photos.length }} uploaded photos</span>
        </div>
      </div>
      <div class="photo-grid">
        <div 
          v-for="(photo, index) in photos" 
          :key="photo.url || index"
          class="photo-item"
          :class="{ 'is-used': getUsageCount(photo.url) > 0 }"
          draggable="true"
          @dragstart="onPhotoDragStart($event, photo)"
          @dragend="onPhotoDragEnd"
        >
          <img :src="photo.url" loading="lazy" alt="Photo item" />
          <div class="usage-badge" v-if="getUsageCount(photo.url) > 0">
            <svg xmlns="http://www.w3.org/2000/svg" width="10" height="10" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="3.5" stroke-linecap="round" stroke-linejoin="round"><polyline points="20 6 9 17 4 12"/></svg>
            <span>{{ getUsageCount(photo.url) }}</span>
          </div>
          <div class="drag-hint-overlay">Drag to page</div>
        </div>
      </div>
    </aside>

    <!-- Main Editor: Canvas -->
    <main class="editor-canvas">
      <!-- Header Bar -->
      <div class="canvas-header">
        <div class="spread-title">
          {{ activeSpreadIndex === -1 ? 'Front Cover' : activeSpreadIndex === -2 ? 'Back Cover' : `Pages ${activeSpreadIndex * 2 + 1} – ${activeSpreadIndex * 2 + 2}` }}
        </div>

        <!-- Preset Auto-Templates Toolbar -->
        <div class="templates-toolbar glass">
          <div class="toolbar-title">
            <svg xmlns="http://www.w3.org/2000/svg" width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><polygon points="12 2 15.09 8.26 22 9.27 17 14.14 18.18 21.02 12 17.77 5.82 21.02 7 14.14 2 9.27 8.91 8.26 12 2"/></svg>
            <span>Auto-Templates</span>
          </div>
          <div class="template-chips">
            <button 
              class="template-chip" 
              :class="{ active: selectedTemplate === 'mix' }"
              @click="applyTemplateStyle('mix')"
              title="Mix of 1, 2, 3 & 4 photo collages"
            >
              ✨ Storybook Mix
            </button>
            <button 
              class="template-chip" 
              :class="{ active: selectedTemplate === 'single' }"
              @click="applyTemplateStyle('single')"
              title="1 Full photo per page"
            >
              📷 Single Photo
            </button>
            <button 
              class="template-chip" 
              :class="{ active: selectedTemplate === 'grid4' }"
              @click="applyTemplateStyle('grid4')"
              title="4-photo 2x2 grid collages"
            >
              🖼️ 2x2 Grid
            </button>
            <button 
              class="template-chip" 
              :class="{ active: selectedTemplate === 'panoramic' }"
              @click="applyTemplateStyle('panoramic')"
              title="Panoramic spreads mixed with collages"
            >
              🌅 Panoramic Spreads
            </button>
          </div>
          <button class="btn-primary btn-autofill" @click="applyTemplateStyle('mix')">
            ⚡ Autofill All
          </button>
        </div>

        <div class="actions">
          <button class="btn-secondary btn-sm" @click="$emit('prev')">Back</button>
          <button class="btn-primary btn-sm" @click="$emit('next')">Review & Order →</button>
        </div>
      </div>

      <div class="canvas-body">
        <!-- Front Cover Editor View -->
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
                placeholder="Enter Front Cover Title..."
                @input="onCoverTitleInput"
              />
              <div class="cover-subtitle">Tizeta-Pages Collection</div>
            </div>
            <div class="drop-hint no-pointer" v-if="!cover.photo && cover.type === 'photo'">
              Drop photo for Front Cover
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

        <!-- Back Cover Editor View -->
        <div v-else-if="activeSpreadIndex === -2" class="cover-editor-view">
          <div 
            class="book-spread cover-spread back-cover-spread"
            :class="{ 'dragging-over': isOverCover }"
            :style="getBackCoverStyle()"
            @dragover.prevent
            @dragenter="isOverCover = true"
            @dragleave="isOverCover = false"
            @drop="onDropOnBackCover"
          >
            <div class="spread-overlay no-pointer" v-if="backCover.type === 'photo'"></div>
            <div class="cover-content" :class="{ 'no-pointer': isDragging }">
              <input 
                type="text" 
                v-model="backCover.text" 
                class="cover-title-input back-title-input" 
                placeholder="Enter Back Cover Closing Text..."
              />
              <div class="cover-subtitle">Printed with ♥ by Tizeta-Pages</div>
            </div>
            <div class="drop-hint no-pointer" v-if="!backCover.photo && backCover.type === 'photo'">
              Drop photo for Back Cover
            </div>
          </div>
          
          <div class="cover-controls glass" :class="{ 'no-pointer': isDragging }">
            <div class="control-group">
              <label>Back Cover Type</label>
              <div class="toggle-group">
                <button :class="{ active: backCover.type === 'photo' }" @click="backCover.type = 'photo'">Photo</button>
                <button :class="{ active: backCover.type === 'color' }" @click="backCover.type = 'color'">Color</button>
              </div>
            </div>
            <div class="control-group" v-if="backCover.type === 'color'">
              <label>Background Color</label>
              <div class="color-picker">
                <div 
                  v-for="c in ['#2D3436', '#7FB069', '#3D5A80', '#EE6C4D', '#98C1D9']" 
                  :key="c"
                  class="color-option"
                  :style="{ backgroundColor: c }"
                  :class="{ active: backCover.color === c }"
                  @click="backCover.color = c"
                ></div>
              </div>
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
            <!-- Single Pages Mode with Collage Support -->
            <template v-if="!bookLayout[activeSpreadIndex].isFull">
              <!-- Left Page -->
              <div 
                class="page left-page" 
                :class="[`layout-${getSinglePage(activeSpreadIndex, 'left').layout}`]"
              >
                <div 
                  v-for="(slot, slotIdx) in getSinglePage(activeSpreadIndex, 'left').slots" 
                  :key="slotIdx"
                  class="collage-slot"
                  :class="{ 
                    'empty': !slot,
                    'dragging-over': draggingOverTarget === `left-${slotIdx}`
                  }"
                  @dragover.prevent
                  @dragenter="draggingOverTarget = `left-${slotIdx}`"
                  @dragleave="draggingOverTarget = null"
                  @drop="onDropOnSlot(activeSpreadIndex, 'left', slotIdx)"
                >
                  <img v-if="slot" :src="slot.url" class="no-pointer" />
                  <div v-else class="drop-label no-pointer">
                    <span class="slot-plus">+</span>
                    <span>Drop Photo</span>
                  </div>
                  <button v-if="slot" class="remove-photo" @click.stop="clearSlotPhoto(activeSpreadIndex, 'left', slotIdx)">×</button>
                </div>

                <!-- Collage Layout Toolbar Picker -->
                <div class="page-layout-picker">
                  <span class="picker-label">Layout:</span>
                  <button 
                    :class="{ active: getSinglePage(activeSpreadIndex, 'left').layout === 'single' }" 
                    @click="setPageLayout(activeSpreadIndex, 'left', 'single')" 
                    title="1 Photo Full"
                  >1</button>
                  <button 
                    :class="{ active: getSinglePage(activeSpreadIndex, 'left').layout === 'split2' }" 
                    @click="setPageLayout(activeSpreadIndex, 'left', 'split2')" 
                    title="2 Photos Split"
                  >2</button>
                  <button 
                    :class="{ active: getSinglePage(activeSpreadIndex, 'left').layout === 'grid3' }" 
                    @click="setPageLayout(activeSpreadIndex, 'left', 'grid3')" 
                    title="3 Photos Collage"
                  >3</button>
                  <button 
                    :class="{ active: getSinglePage(activeSpreadIndex, 'left').layout === 'grid4' }" 
                    @click="setPageLayout(activeSpreadIndex, 'left', 'grid4')" 
                    title="4 Photos 2x2 Grid"
                  >4</button>
                </div>
              </div>

              <!-- Right Page -->
              <div 
                class="page right-page" 
                :class="[`layout-${getSinglePage(activeSpreadIndex, 'right').layout}`]"
              >
                <div 
                  v-for="(slot, slotIdx) in getSinglePage(activeSpreadIndex, 'right').slots" 
                  :key="slotIdx"
                  class="collage-slot"
                  :class="{ 
                    'empty': !slot,
                    'dragging-over': draggingOverTarget === `right-${slotIdx}`
                  }"
                  @dragover.prevent
                  @dragenter="draggingOverTarget = `right-${slotIdx}`"
                  @dragleave="draggingOverTarget = null"
                  @drop="onDropOnSlot(activeSpreadIndex, 'right', slotIdx)"
                >
                  <img v-if="slot" :src="slot.url" class="no-pointer" />
                  <div v-else class="drop-label no-pointer">
                    <span class="slot-plus">+</span>
                    <span>Drop Photo</span>
                  </div>
                  <button v-if="slot" class="remove-photo" @click.stop="clearSlotPhoto(activeSpreadIndex, 'right', slotIdx)">×</button>
                </div>

                <!-- Collage Layout Toolbar Picker -->
                <div class="page-layout-picker">
                  <span class="picker-label">Layout:</span>
                  <button 
                    :class="{ active: getSinglePage(activeSpreadIndex, 'right').layout === 'single' }" 
                    @click="setPageLayout(activeSpreadIndex, 'right', 'single')" 
                    title="1 Photo Full"
                  >1</button>
                  <button 
                    :class="{ active: getSinglePage(activeSpreadIndex, 'right').layout === 'split2' }" 
                    @click="setPageLayout(activeSpreadIndex, 'right', 'split2')" 
                    title="2 Photos Split"
                  >2</button>
                  <button 
                    :class="{ active: getSinglePage(activeSpreadIndex, 'right').layout === 'grid3' }" 
                    @click="setPageLayout(activeSpreadIndex, 'right', 'grid3')" 
                    title="3 Photos Collage"
                  >3</button>
                  <button 
                    :class="{ active: getSinglePage(activeSpreadIndex, 'right').layout === 'grid4' }" 
                    @click="setPageLayout(activeSpreadIndex, 'right', 'grid4')" 
                    title="4 Photos 2x2 Grid"
                  >4</button>
                </div>
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
          </div>
          
          <div class="spread-tools">
            <button 
              class="btn-secondary btn-sm" 
              :class="{ active: bookLayout[activeSpreadIndex].isFull }"
              @click="toggleFullSpread(activeSpreadIndex)"
            >
              {{ bookLayout[activeSpreadIndex].isFull ? 'Switch to 2 Pages' : 'Make Full Spread' }}
            </button>
            <p class="tool-hint">
              Tip: Choose page layout options (1, 2, 3, 4 photos) or click "Autofill All" for instant 1-click book design!
            </p>
          </div>
        </div>
      </div>
    </main>

    <!-- Right Sidebar: Page Navigator -->
    <aside class="page-navigator">
      <div class="sidebar-header">
        <h3>Book Spreads</h3>
      </div>
      <div class="nav-list">
        <!-- Front Cover Item -->
        <div 
          class="nav-item cover-item" 
          :class="{ active: activeSpreadIndex === -1 }"
          @click="activeSpreadIndex = -1"
        >
          <div class="nav-preview cover-preview" :style="getCoverStyle()"></div>
          <div class="nav-label">Front Cover</div>
        </div>

        <!-- Inner Spreads -->
        <div 
          v-for="(spread, index) in bookLayout" 
          :key="index"
          class="nav-item"
          :class="{ active: activeSpreadIndex === index }"
          @click="activeSpreadIndex = index"
        >
          <div class="nav-preview spread-preview">
            <template v-if="!spread.isFull">
              <div class="p-left">
                <img v-if="getSinglePage(index, 'left').slots[0]" :src="getSinglePage(index, 'left').slots[0].url" />
              </div>
              <div class="p-right">
                <img v-if="getSinglePage(index, 'right').slots[0]" :src="getSinglePage(index, 'right').slots[0].url" />
              </div>
            </template>
            <div v-else class="p-full">
              <img v-if="spread.full" :src="spread.full.url" />
            </div>
          </div>
          <div class="nav-label">Pages {{ index * 2 + 1 }}-{{ index * 2 + 2 }}</div>
        </div>

        <!-- Back Cover Item -->
        <div 
          class="nav-item cover-item" 
          :class="{ active: activeSpreadIndex === -2 }"
          @click="activeSpreadIndex = -2"
        >
          <div class="nav-preview cover-preview" :style="getBackCoverStyle()"></div>
          <div class="nav-label">Back Cover</div>
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

const activeSpreadIndex = ref(-1); // -1 Front Cover, -2 Back Cover
const draggedPhoto = ref(null);
const isDragging = ref(false);
const isOverCover = ref(false);
const draggingOverTarget = ref(null);
const selectedTemplate = ref('mix');

const backCover = ref({
  type: 'color',
  color: '#2D3436',
  photo: null,
  text: 'Tizeta-Pages • Memory Collection'
});

// Initialize Book Layout from props.photos
const bookLayout = ref([]);

const initLayout = () => {
  applyTemplateStyle('mix');
};

onMounted(() => {
  initLayout();
});

const getSinglePage = (spreadIndex, side) => {
  if (!bookLayout.value[spreadIndex]) return { layout: 'single', slots: [null] };
  const page = bookLayout.value[spreadIndex][side];
  if (!page) {
    return { layout: 'single', slots: [null] };
  }
  if (page.slots) {
    return page;
  }
  return { layout: 'single', slots: [page || null] };
};

const setPageLayout = (spreadIndex, side, layoutType) => {
  const current = getSinglePage(spreadIndex, side);
  const targetCount = layoutType === 'grid4' ? 4 : layoutType === 'grid3' ? 3 : layoutType === 'split2' ? 2 : 1;
  
  const newSlots = [];
  for (let i = 0; i < targetCount; i++) {
    newSlots.push(current.slots[i] || null);
  }
  
  bookLayout.value[spreadIndex][side] = {
    layout: layoutType,
    slots: newSlots
  };
  bookLayout.value[spreadIndex].isFull = false;
  syncPhotos();
};

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

const onDropOnBackCover = () => {
  if (draggedPhoto.value) {
    backCover.value.photo = draggedPhoto.value.url;
    backCover.value.type = 'photo';
    onPhotoDragEnd();
  }
};

const onDropOnSlot = (spreadIndex, side, slotIndex) => {
  if (!draggedPhoto.value) return;
  
  const page = getSinglePage(spreadIndex, side);
  page.slots[slotIndex] = draggedPhoto.value;
  bookLayout.value[spreadIndex][side] = { ...page };
  bookLayout.value[spreadIndex].isFull = false;
  
  syncPhotos();
  draggedPhoto.value = null;
};

const onDropOnPage = (index, side) => {
  if (!draggedPhoto.value) return;
  if (side === 'full') {
    bookLayout.value[index].full = draggedPhoto.value;
    bookLayout.value[index].isFull = true;
  }
  syncPhotos();
  draggedPhoto.value = null;
};

const clearSlotPhoto = (spreadIndex, side, slotIndex) => {
  const page = getSinglePage(spreadIndex, side);
  page.slots[slotIndex] = null;
  bookLayout.value[spreadIndex][side] = { ...page };
  syncPhotos();
};

const clearPhoto = (index, side) => {
  if (side === 'full') bookLayout.value[index].full = null;
  syncPhotos();
};

const toggleFullSpread = (index) => {
  bookLayout.value[index].isFull = !bookLayout.value[index].isFull;
  syncPhotos();
};

const addSpread = () => {
  bookLayout.value.push({ 
    left: { layout: 'single', slots: [null] }, 
    right: { layout: 'single', slots: [null] }, 
    isFull: false, 
    full: null 
  });
};

const applyTemplateStyle = (templateType) => {
  selectedTemplate.value = templateType;
  let photoIdx = 0;
  const spreads = [];
  const totalPhotos = props.photos || [];
  
  const numSpreads = 12; // 24 pages + cover
  
  for (let s = 0; s < numSpreads; s++) {
    let isFull = false;
    let full = null;
    
    if (templateType === 'panoramic' && s % 3 === 2 && photoIdx < totalPhotos.length) {
      isFull = true;
      full = totalPhotos[photoIdx++];
    }
    
    // Left Page Layout
    let leftLayout = 'single';
    if (templateType === 'grid4') leftLayout = 'grid4';
    else if (templateType === 'mix') {
      const layouts = ['single', 'split2', 'grid3', 'grid4'];
      leftLayout = layouts[s % layouts.length];
    }
    
    const leftCount = leftLayout === 'grid4' ? 4 : leftLayout === 'grid3' ? 3 : leftLayout === 'split2' ? 2 : 1;
    const leftSlots = [];
    for (let i = 0; i < leftCount; i++) {
      leftSlots.push(totalPhotos[photoIdx++] || null);
    }
    
    // Right Page Layout
    let rightLayout = 'single';
    if (templateType === 'grid4') rightLayout = 'grid4';
    else if (templateType === 'mix') {
      const layouts = ['split2', 'grid3', 'single', 'grid4'];
      rightLayout = layouts[(s + 1) % layouts.length];
    }
    
    const rightCount = rightLayout === 'grid4' ? 4 : rightLayout === 'grid3' ? 3 : rightLayout === 'split2' ? 2 : 1;
    const rightSlots = [];
    for (let i = 0; i < rightCount; i++) {
      rightSlots.push(totalPhotos[photoIdx++] || null);
    }
    
    spreads.push({
      isFull,
      full,
      left: { layout: leftLayout, slots: leftSlots },
      right: { layout: rightLayout, slots: rightSlots }
    });
  }
  
  bookLayout.value = spreads;
  syncPhotos();
};

const syncPhotos = () => {
  const usedPhotos = [];
  bookLayout.value.forEach(spread => {
    if (spread.isFull) {
      if (spread.full) usedPhotos.push(spread.full);
    } else {
      if (spread.left?.slots) {
        spread.left.slots.forEach(slot => { if (slot) usedPhotos.push(slot); });
      }
      if (spread.right?.slots) {
        spread.right.slots.forEach(slot => { if (slot) usedPhotos.push(slot); });
      }
    }
  });
  emit('update:photos', usedPhotos);
};

const onCoverTitleInput = (e) => {
  emit('update:cover', { ...props.cover, title: e.target.value });
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

const getBackCoverStyle = () => {
  if (backCover.value.type === 'color') {
    return { backgroundColor: backCover.value.color };
  }
  return { 
    backgroundImage: backCover.value.photo ? `url(${backCover.value.photo})` : 'none',
    backgroundSize: 'cover',
    backgroundPosition: 'center',
    backgroundColor: '#2D3436'
  };
};

const getUsageCount = (url) => {
  let count = 0;
  if (props.cover.photo === url) count++;
  if (backCover.value.photo === url) count++;
  bookLayout.value.forEach(s => {
    if (s.isFull && s.full?.url === url) count++;
    else {
      if (s.left?.slots) {
        s.left.slots.forEach(slot => { if (slot?.url === url) count++; });
      }
      if (s.right?.slots) {
        s.right.slots.forEach(slot => { if (slot?.url === url) count++; });
      }
    }
  });
  return count;
};
</script>

<style scoped>
.editor-wrapper {
  display: grid;
  grid-template-columns: 290px 1fr 240px;
  height: calc(100vh - 160px);
  min-height: 550px;
  margin-bottom: 30px;
  background: white;
  border-radius: var(--radius-lg);
  border: 1px solid var(--border-color);
  overflow: hidden;
  box-shadow: 0 10px 40px rgba(0,0,0,0.05);
}

.photo-library {
  width: 290px;
  min-width: 290px;
  background: #f9faf9;
  display: flex;
  flex-direction: column;
  height: 100%;
  overflow: hidden;
  border-right: 1px solid var(--border-color);
}

.page-navigator {
  background: #f9faf9;
  display: flex;
  flex-direction: column;
  height: 100%;
  overflow: hidden;
  border-left: 1px solid var(--border-color);
}

.sidebar-header {
  padding: 18px 20px;
  background: white;
  border-bottom: 1px solid var(--border-color);
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.sidebar-header h3 {
  font-size: 0.95rem;
  font-weight: 700;
  color: var(--text-main);
  margin: 0 0 2px 0;
  letter-spacing: -0.2px;
}

.count {
  font-size: 0.78rem;
  color: var(--text-muted);
  font-weight: 500;
}

.photo-grid {
  flex: 1 1 0%;
  height: 0;
  min-height: 0;
  overflow-y: auto;
  padding: 16px 16px 40px;
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 14px;
  align-content: flex-start;
  scrollbar-width: thin;
  scrollbar-color: var(--primary) transparent;
}

.photo-item {
  width: 100%;
  aspect-ratio: 1 / 1;
  min-height: 105px;
  border-radius: 10px;
  overflow: hidden;
  cursor: grab;
  position: relative;
  border: 1.5px solid var(--border-color);
  background: white;
  box-shadow: 0 2px 8px rgba(0,0,0,0.03);
  transition: all 0.25s cubic-bezier(0.4, 0, 0.2, 1);
}

.photo-item:hover {
  transform: translateY(-3px);
  box-shadow: 0 8px 18px rgba(0,0,0,0.08);
  border-color: var(--primary);
}

.photo-item.is-used {
  border-color: rgba(127, 176, 105, 0.5);
}

.photo-item:active {
  cursor: grabbing;
}

.photo-item img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}

.usage-badge {
  position: absolute;
  top: 6px;
  right: 6px;
  background: rgba(46, 125, 50, 0.9);
  backdrop-filter: blur(4px);
  color: white;
  padding: 2px 7px;
  border-radius: 12px;
  font-size: 0.7rem;
  font-weight: 700;
  display: flex;
  align-items: center;
  gap: 3px;
  box-shadow: 0 2px 6px rgba(0,0,0,0.2);
}

.drag-hint-overlay {
  position: absolute;
  inset: 0;
  background: rgba(0, 0, 0, 0.35);
  color: white;
  font-size: 0.72rem;
  font-weight: 600;
  display: flex;
  align-items: center;
  justify-content: center;
  opacity: 0;
  transition: opacity 0.2s;
  pointer-events: none;
}

.photo-item:hover .drag-hint-overlay {
  opacity: 1;
}

.editor-canvas {
  display: flex;
  flex-direction: column;
  background: #f0f2f0;
  position: relative;
  height: 100%;
  overflow: hidden;
}

.canvas-header {
  padding: 12px 24px;
  background: white;
  border-bottom: 1px solid var(--border-color);
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 16px;
}

.spread-title {
  font-weight: 700;
  font-size: 1rem;
  color: var(--text-main);
  white-space: nowrap;
}

.templates-toolbar {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 4px 12px;
  background: #f4f7f4;
  border-radius: 50px;
  border: 1px solid var(--border-color);
}

.toolbar-title {
  display: flex;
  align-items: center;
  gap: 6px;
  font-size: 0.78rem;
  font-weight: 700;
  color: var(--text-main);
}

.template-chips {
  display: flex;
  gap: 6px;
}

.template-chip {
  background: white;
  border: 1px solid var(--border-color);
  border-radius: 50px;
  padding: 4px 10px;
  font-size: 0.75rem;
  font-weight: 600;
  color: var(--text-muted);
  cursor: pointer;
  transition: all 0.2s;
}

.template-chip.active, .template-chip:hover {
  background: var(--primary);
  color: white;
  border-color: var(--primary);
}

.btn-autofill {
  padding: 6px 14px;
  font-size: 0.78rem;
  border-radius: 50px;
  white-space: nowrap;
}

.actions {
  display: flex;
  gap: 10px;
}

.canvas-body {
  flex: 1 1 0%;
  height: 0;
  min-height: 0;
  padding: 28px;
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: auto;
}

.cover-editor-view, .pages-editor-view {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 20px;
  width: 100%;
  max-width: 860px;
}

.book-spread {
  width: 100%;
  aspect-ratio: 1.6;
  max-height: 480px;
  background: white;
  border-radius: 4px 12px 12px 4px;
  box-shadow: 0 15px 45px rgba(0,0,0,0.1);
  display: flex;
  position: relative;
  overflow: hidden;
}

.cover-spread {
  border-radius: 4px 14px 14px 4px;
  display: flex;
  flex-direction: column;
  justify-content: flex-end;
  padding: 40px;
  position: relative;
  background-size: cover;
  background-position: center;
}

.back-cover-spread {
  background-color: #2D3436;
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
  border-bottom: 2px dashed rgba(255,255,255,0.6);
  color: white;
  font-size: 2.2rem;
  font-weight: 700;
  width: 100%;
  padding: 4px 0;
  text-shadow: 0 2px 10px rgba(0,0,0,0.5);
}

.back-title-input {
  font-size: 1.6rem;
}

.cover-title-input:focus {
  outline: none;
  border-bottom-style: solid;
  border-bottom-color: white;
}

.cover-subtitle {
  color: rgba(255,255,255,0.8);
  font-size: 0.85rem;
  text-transform: uppercase;
  letter-spacing: 2px;
  margin-top: 8px;
}

.drop-hint {
  position: absolute;
  inset: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  color: rgba(0,0,0,0.4);
  font-size: 1.2rem;
  font-weight: 600;
  border: 2px dashed rgba(0,0,0,0.2);
  margin: 20px;
  border-radius: 8px;
}

/* Page Collage Layout Classes */
.page {
  flex: 1;
  position: relative;
  background: white;
  display: grid;
  gap: 6px;
  padding: 8px;
  overflow: hidden;
  border-right: 1px solid #eee;
}

.left-page {
  border-right: 1px solid var(--border-color);
}

.page.layout-single {
  grid-template-columns: 1fr;
  grid-template-rows: 1fr;
  padding: 0;
}

.page.layout-split2 {
  grid-template-columns: 1fr;
  grid-template-rows: 1fr 1fr;
}

.page.layout-grid3 {
  grid-template-columns: 1fr 1fr;
  grid-template-rows: 1fr 1fr;
}

.page.layout-grid3 .collage-slot:nth-child(1) {
  grid-row: span 2;
}

.page.layout-grid4 {
  grid-template-columns: 1fr 1fr;
  grid-template-rows: 1fr 1fr;
}

.collage-slot {
  position: relative;
  background: #f6f8f6;
  border-radius: 4px;
  overflow: hidden;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.2s ease;
  border: 1.5px dashed transparent;
}

.collage-slot.empty {
  border-color: #d1d5db;
}

.collage-slot.dragging-over {
  border-color: var(--primary);
  background: rgba(127, 176, 105, 0.15);
  box-shadow: inset 0 0 0 2px var(--primary);
}

.collage-slot img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.drop-label {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 4px;
  color: var(--text-muted);
  font-size: 0.8rem;
  font-weight: 600;
}

.slot-plus {
  font-size: 1.2rem;
  font-weight: 700;
  color: var(--primary);
}

.page-layout-picker {
  position: absolute;
  bottom: 8px;
  left: 50%;
  transform: translateX(-50%);
  background: rgba(0, 0, 0, 0.8);
  backdrop-filter: blur(8px);
  padding: 4px 10px;
  border-radius: 30px;
  display: flex;
  align-items: center;
  gap: 6px;
  opacity: 0;
  transition: opacity 0.2s ease;
  z-index: 10;
  box-shadow: 0 4px 12px rgba(0,0,0,0.15);
}

.page:hover .page-layout-picker {
  opacity: 1;
}

.picker-label {
  color: rgba(255,255,255,0.7);
  font-size: 0.7rem;
  font-weight: 700;
  margin-right: 2px;
}

.page-layout-picker button {
  background: transparent;
  color: rgba(255, 255, 255, 0.7);
  border: none;
  width: 22px;
  height: 22px;
  border-radius: 50%;
  font-size: 0.75rem;
  font-weight: 700;
  cursor: pointer;
}

.page-layout-picker button.active,
.page-layout-picker button:hover {
  background: var(--primary);
  color: white;
}

.full-spread-container {
  width: 100%;
  height: 100%;
  position: relative;
  background: #f6f8f6;
}

.full-spread-container img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.remove-photo {
  position: absolute;
  top: 8px;
  right: 8px;
  width: 24px;
  height: 24px;
  background: rgba(0,0,0,0.6);
  color: white;
  border-radius: 50%;
  border: none;
  font-size: 14px;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 4;
}

.remove-photo:hover {
  background: rgba(220, 53, 69, 0.9);
}

.cover-controls {
  display: flex;
  gap: 24px;
  padding: 14px 24px;
  border-radius: var(--radius-md);
  background: white;
  border: 1px solid var(--border-color);
}

.control-group {
  display: flex;
  flex-direction: column;
  gap: 6px;
}

.control-group label {
  font-size: 0.8rem;
  font-weight: 600;
  color: var(--text-muted);
}

.toggle-group {
  display: flex;
  background: #eee;
  padding: 2px;
  border-radius: 6px;
}

.toggle-group button {
  padding: 5px 12px;
  border: none;
  background: transparent;
  border-radius: 4px;
  font-size: 0.82rem;
  cursor: pointer;
}

.toggle-group button.active {
  background: white;
  font-weight: 600;
  box-shadow: 0 2px 4px rgba(0,0,0,0.05);
}

.color-picker {
  display: flex;
  gap: 8px;
}

.color-option {
  width: 24px;
  height: 24px;
  border-radius: 50%;
  cursor: pointer;
  border: 2px solid transparent;
}

.color-option.active {
  border-color: var(--primary);
  transform: scale(1.1);
}

select {
  padding: 6px 12px;
  border-radius: 6px;
  border: 1px solid var(--border-color);
  font-family: inherit;
}

.spread-tools {
  display: flex;
  align-items: center;
  gap: 16px;
}

.tool-hint {
  font-size: 0.85rem;
  color: var(--text-muted);
}

.nav-list {
  flex: 1 1 0%;
  height: 0;
  min-height: 0;
  overflow-y: auto;
  padding: 16px;
  display: flex;
  flex-direction: column;
  gap: 16px;
  scrollbar-width: thin;
  scrollbar-color: var(--primary) transparent;
}

.nav-item {
  cursor: pointer;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 6px;
}

.nav-preview {
  width: 100%;
  aspect-ratio: 1.6;
  background: white;
  border-radius: 4px;
  border: 1px solid var(--border-color);
  overflow: hidden;
  transition: var(--transition);
}

.nav-item.active .nav-preview {
  border-color: var(--primary);
  box-shadow: 0 0 0 2px var(--primary);
}

.spread-preview {
  display: flex;
}

.p-left, .p-right {
  flex: 1;
  height: 100%;
  background: #eee;
  border-right: 1px solid #fff;
}

.p-left img, .p-right img, .p-full img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.p-full {
  width: 100%;
  height: 100%;
}

.nav-label {
  font-size: 0.75rem;
  color: var(--text-muted);
}

.add-spread-btn {
  width: 100%;
  padding: 10px;
  background: transparent;
  border: 1px dashed var(--border-color);
  border-radius: 6px;
  color: var(--text-muted);
  font-weight: 600;
  cursor: pointer;
}

.add-spread-btn:hover {
  border-color: var(--primary);
  color: var(--primary);
}

.no-pointer {
  pointer-events: none;
}
</style>

<template>
  <div class="step-container">
    <div class="step-header">
      <h2>Step 3: Arrange your pages</h2>
      <p>We've automatically layout out your photos. Drag to reorder if you like.</p>
    </div>

    <div class="pages-grid">
      <!-- Cover Page -->
      <div class="page-spread cover-spread">
        <div class="page-side cover-front" :style="getCoverStyle()">
           <div class="cover-title">{{ cover.title || 'Untitled Book' }}</div>
        </div>
        <div class="spread-info">Front Cover</div>
      </div>

      <!-- Content Spreads -->
      <div 
        v-for="(spread, sIndex) in spreads" 
        :key="sIndex" 
        class="page-spread"
        draggable="true"
        @dragstart="startDrag(sIndex)"
        @dragover.prevent
        @drop="drop(sIndex)"
      >
        <div class="spread-inner">
          <div class="page-side left-page">
            <img :src="spread[0]?.url" />
          </div>
          <div class="page-side right-page">
            <img v-if="spread[1]" :src="spread[1]?.url" />
            <div v-else class="empty-page">Empty</div>
          </div>
        </div>
        <div class="spread-info">Pages {{ sIndex * 2 + 1 }} - {{ sIndex * 2 + 2 }}</div>
      </div>
    </div>

    <div class="step-footer">
      <div class="info-badge">Minimum 25 pages met ({{ photos.length }} photos)</div>
      <div class="navigation-actions">
        <button class="btn-secondary" @click="$emit('prev')">Back</button>
        <button class="btn-primary" @click="$emit('next')">Order Book</button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { computed, ref } from 'vue';

const props = defineProps(['photos', 'cover']);
const emit = defineEmits(['update:photos', 'next', 'prev']);

const dragsIndex = ref(null);

const spreads = computed(() => {
  const s = [];
  for (let i = 0; i < props.photos.length; i += 2) {
    s.push([props.photos[i], props.photos[i + 1]]);
  }
  return s;
});

const getCoverStyle = () => {
  if (props.cover.type === 'color') {
    return { backgroundColor: props.cover.color };
  }
  return { 
    backgroundImage: `url(${props.cover.photo})`,
    backgroundSize: 'cover',
    backgroundPosition: 'center'
  };
};

const startDrag = (index) => {
  dragsIndex.value = index;
};

const drop = (index) => {
  const newPhotos = [...props.photos];
  // Logic to swap/move spreads is more complex, 
  // let's just do a simple move of the two photos in the spread
  const fromIdx = dragsIndex.value * 2;
  const toIdx = index * 2;
  
  const moved = newPhotos.splice(fromIdx, 2);
  newPhotos.splice(toIdx, 0, ...moved);
  
  emit('update:photos', newPhotos);
};
</script>

<style scoped>
.step-container {
  padding: 40px 0;
}

.step-header {
  text-align: center;
  margin-bottom: 60px;
}

.pages-grid {
  display: flex;
  flex-direction: column;
  gap: 40px;
  max-width: 900px;
  margin: 0 auto;
}

.page-spread {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 12px;
}

.spread-inner {
  display: flex;
  width: 100%;
  aspect-ratio: 1.6;
  background: white;
  box-shadow: 0 10px 30px rgba(0,0,0,0.05);
  border-radius: 4px;
  overflow: hidden;
  border: 1px solid var(--border-color);
}

.page-side {
  flex: 1;
  position: relative;
  overflow: hidden;
  padding: 20px;
  background: white;
}

.left-page {
  border-right: 1px solid rgba(0,0,0,0.05);
}

.page-side img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  border-radius: 2px;
}

.cover-front {
  width: 50%;
  aspect-ratio: 0.8;
  display: flex;
  align-items: flex-end;
  padding: 40px;
}

.cover-title {
  color: white;
  font-family: 'Outfit', sans-serif;
  font-weight: 700;
  font-size: 2rem;
  text-transform: uppercase;
  text-shadow: 0 2px 10px rgba(0,0,0,0.2);
}

.spread-info {
  font-size: 0.85rem;
  color: var(--text-muted);
  font-weight: 500;
}

.empty-page {
  width: 100%;
  height: 100%;
  background: #f8f9f8;
  display: flex;
  align-items: center;
  justify-content: center;
  color: var(--text-muted);
  border: 1px dashed var(--border-color);
}

.step-footer {
  margin-top: 80px;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 24px;
}

.info-badge {
  background: var(--bg-color);
  padding: 8px 16px;
  border-radius: 50px;
  font-weight: 600;
  font-size: 0.9rem;
  color: var(--primary);
  border: 1px solid var(--primary);
}

.navigation-actions {
  display: flex;
  gap: 16px;
}
</style>

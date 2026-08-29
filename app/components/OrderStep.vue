<template>
  <div class="step-container">
    <div v-if="orderCompleted" class="success-card">
      <div class="success-icon">🎉</div>
      <h2>Order Placed Successfully!</h2>
      <p>Thank you for ordering your custom memory book. We're now preparing your pages for archival printing.</p>
      <div class="order-ref">Order Ref: <strong>#TZ-{{ Math.floor(100000 + Math.random() * 900000) }}</strong></div>
      <button class="btn-primary" @click="$emit('complete')">Back to Home</button>
    </div>

    <div v-else>
      <div class="step-header">
        <span class="badge-step">Step 3 of 3</span>
        <h2>Final Step: Order your book</h2>
        <p>Review your book specifications, enter your shipping details, and complete your order.</p>
      </div>

      <!-- 3D Flipbook Section -->
      <div class="flipbook-section">
        <div class="flipbook-wrapper" @touchstart="onTouchStart" @touchend="onTouchEnd">
          
          <div 
            v-for="(leaf, index) in leaves" 
            :key="index"
            class="book-leaf"
            :class="{ flipped: index < currentPage }"
            :style="{ zIndex: index < currentPage ? index : leaves.length - index }"
          >
            <!-- FRONT OF LEAF -->
            <div class="leaf-face leaf-front" @click="turnPage(index, 'forward')">
              <template v-if="leaf.front.type === 'cover'">
                <div class="cover-spread" :style="getCoverStyle(leaf.front.data)">
                  <div class="spread-overlay" v-if="leaf.front.data.type === 'photo'"></div>
                  <div class="cover-content" :style="{ fontFamily: leaf.front.data.font }">
                    <div class="cover-title">{{ leaf.front.data.title || 'Your Title Here' }}</div>
                    <div class="cover-subtitle">Tizeta-Pages Collection</div>
                  </div>
                </div>
              </template>
              <template v-else-if="leaf.front.data">
                <div v-if="leaf.front.isFull" class="full-spread-bg" :style="{ backgroundImage: `url(${leaf.front.full?.url})`, backgroundPosition: 'right center' }"></div>
                <div v-else class="page right-page" :class="[`layout-${leaf.front.data.layout}`]">
                  <div class="collage-slot" v-for="(photo, sIdx) in leaf.front.data.slots" :key="sIdx">
                    <img v-if="photo" :src="photo.url" />
                  </div>
                </div>
              </template>
            </div>

            <!-- BACK OF LEAF -->
            <div class="leaf-face leaf-back" @click="turnPage(index, 'backward')">
              <template v-if="leaf.back.type === 'backCover'">
                <div class="cover-spread" :style="getBackCoverStyle(leaf.back.data)">
                  <div class="back-cover-slots" v-if="leaf.back.data.type === 'photo-2'">
                    <div class="collage-slot"><img v-if="leaf.back.data.photo" :src="leaf.back.data.photo" /></div>
                    <div class="collage-slot"><img v-if="leaf.back.data.photo2" :src="leaf.back.data.photo2" /></div>
                  </div>
                  <div class="spread-overlay" v-if="leaf.back.data.type && leaf.back.data.type.startsWith('photo')"></div>
                  <div class="cover-content">
                    <div class="cover-title" style="font-size: 1.5rem; text-shadow: none;">{{ leaf.back.data.text }}</div>
                  </div>
                </div>
              </template>
              <template v-else-if="leaf.back.data">
                <div v-if="leaf.back.isFull" class="full-spread-bg" :style="{ backgroundImage: `url(${leaf.back.full?.url})`, backgroundPosition: 'left center' }"></div>
                <div v-else class="page left-page" :class="[`layout-${leaf.back.data.layout}`]">
                  <div class="collage-slot" v-for="(photo, sIdx) in leaf.back.data.slots" :key="sIdx">
                    <img v-if="photo" :src="photo.url" />
                  </div>
                </div>
              </template>
            </div>
          </div>
        </div>

        <div class="flipbook-controls">
          <button class="btn-secondary btn-sm" @click="prevPage" :disabled="currentPage === 0">← Prev</button>
          <span class="page-indicator">Spread {{ currentPage }} of {{ leaves.length }}</span>
          <button class="btn-secondary btn-sm" @click="nextPage" :disabled="currentPage === leaves.length">Next →</button>
        </div>
      </div>

      <div class="summary-layout">
        <div class="details-panel">
          <div class="summary-card">
            <h3>Order Summary</h3>
            <div class="summary-row">
              <span>Format</span>
              <strong>Premium Hardcover Book</strong>
            </div>
            <div class="summary-row">
              <span>Total Pages</span>
              <span>{{ photos.length > 0 ? photos.length : 24 }} Pages</span>
            </div>
            <div class="summary-row total">
              <span>Total Price</span>
              <span class="price-val">$48.00 <small>(Free Shipping)</small></span>
            </div>
          </div>
        </div>

        <div class="delivery-card">
          <h3>Delivery Information</h3>
          <form class="delivery-form" @submit.prevent="completeOrder">
            <div class="form-row">
              <input type="text" v-model="form.name" placeholder="Full Name" required />
            </div>
            <div class="form-row">
              <input type="email" v-model="form.email" placeholder="Email Address" required />
            </div>
            <div class="form-row">
              <input type="text" v-model="form.address" placeholder="Shipping Address" required />
            </div>
            <button class="btn-primary full-width btn-order" type="submit" style="margin-top: 10px;">
              <span>Place Order & Pay ($48.00)</span>
            </button>
            <button type="button" class="btn-text" @click="$emit('prev')">← Back to Design</button>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';

const props = defineProps(['photos', 'cover', 'backCover', 'bookLayout']);
const emit = defineEmits(['prev', 'complete']);

const orderCompleted = ref(false);
const currentPage = ref(0);
let touchStartX = 0;

const form = ref({
  name: '',
  email: '',
  address: '',
  city: '',
  zip: ''
});

// Create leaves for 3D Flipbook
const leaves = computed(() => {
  const arr = [];
  
  // Leaf 0: Front Cover (Front) / Spread 0 Left (Back)
  arr.push({
    front: { type: 'cover', data: props.cover },
    back: { type: 'page', data: props.bookLayout[0]?.left, isFull: props.bookLayout[0]?.isFull, full: props.bookLayout[0]?.full }
  });
  
  // Middle Leaves
  for(let i=1; i<props.bookLayout.length; i++) {
    arr.push({
      front: { type: 'page', data: props.bookLayout[i-1]?.right, isFull: props.bookLayout[i-1]?.isFull, full: props.bookLayout[i-1]?.full },
      back: { type: 'page', data: props.bookLayout[i]?.left, isFull: props.bookLayout[i]?.isFull, full: props.bookLayout[i]?.full }
    });
  }
  
  // Last Leaf: Last Spread Right (Front) / Back Cover (Back)
  const lastSpread = props.bookLayout[props.bookLayout.length - 1];
  arr.push({
    front: { type: 'page', data: lastSpread?.right, isFull: lastSpread?.isFull, full: lastSpread?.full },
    back: { type: 'backCover', data: props.backCover }
  });
  
  return arr;
});

const turnPage = (index, direction) => {
  if (direction === 'forward') {
    currentPage.value = index + 1;
  } else {
    currentPage.value = index;
  }
};

const prevPage = () => { if (currentPage.value > 0) currentPage.value--; };
const nextPage = () => { if (currentPage.value < leaves.value.length) currentPage.value++; };

const onTouchStart = (e) => {
  touchStartX = e.changedTouches[0].screenX;
};

const onTouchEnd = (e) => {
  const touchEndX = e.changedTouches[0].screenX;
  if (touchEndX < touchStartX - 50) nextPage();
  if (touchEndX > touchStartX + 50) prevPage();
};

const getCoverStyle = (coverData) => {
  if (coverData.type === 'color') return { backgroundColor: coverData.color };
  return { 
    backgroundImage: `url(${coverData.photo || 'https://images.unsplash.com/photo-1544144433-d50aff500b91?w=600'})`,
    backgroundSize: 'cover',
    backgroundPosition: 'center'
  };
};

const getBackCoverStyle = (backData) => {
  if (backData.type === 'color' || backData.type === 'photo-2') return { backgroundColor: backData.color || '#2D3436' };
  return { 
    backgroundImage: backData.photo ? `url(${backData.photo})` : 'none',
    backgroundSize: 'cover',
    backgroundPosition: 'center'
  };
};

const completeOrder = () => {
  orderCompleted.value = true;
};
</script>

<style scoped>
.step-container {
  max-width: 960px;
  margin: 0 auto;
  padding: 30px 20px 100px;
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

.step-header {
  text-align: center;
  margin-bottom: 40px;
}

.step-header h2 {
  font-size: 2.4rem;
  font-weight: 700;
  margin-bottom: 8px;
}

.step-header p {
  color: var(--text-muted);
  font-size: 1.05rem;
}

.summary-layout {
  display: grid;
  grid-template-columns: 1fr 380px;
  gap: 40px;
}

.summary-card, .delivery-card {
  background: white;
  padding: 28px 32px;
  border-radius: var(--radius-lg);
  border: 1px solid var(--border-color);
  margin-bottom: 24px;
  box-shadow: 0 4px 20px rgba(0,0,0,0.03);
}

h3 {
  margin-bottom: 20px;
  font-size: 1.2rem;
  font-weight: 700;
  color: var(--text-main);
}

.summary-row {
  display: flex;
  justify-content: space-between;
  margin-bottom: 14px;
  font-size: 0.95rem;
  color: var(--text-muted);
}

.summary-row strong {
  color: var(--text-main);
}

.summary-row.total {
  color: var(--text-main);
  font-weight: 700;
  font-size: 1.25rem;
  margin-top: 16px;
  align-items: center;
}

.price-val small {
  font-size: 0.75rem;
  font-weight: 500;
  color: var(--primary);
}

.divider {
  height: 1px;
  background: var(--border-color);
  margin: 20px 0;
}

.delivery-form {
  display: flex;
  flex-direction: column;
  gap: 16px;
}

.form-row input {
  width: 100%;
  padding: 12px 16px;
  border: 1px solid var(--border-color);
  border-radius: var(--radius-sm);
  font-family: inherit;
  font-size: 0.95rem;
  transition: border-color 0.2s;
}

.form-row input:focus {
  outline: none;
  border-color: var(--primary);
}

.form-row.split {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 16px;
}

.flipbook-section {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-bottom: 60px;
  background: #f0f2f0;
  padding: 40px 20px;
  border-radius: var(--radius-lg);
}

.flipbook-wrapper {
  perspective: 2500px;
  width: 100%;
  max-width: 800px;
  aspect-ratio: 1.6;
  margin: 0 auto;
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
}

.book-spine-bg {
  position: absolute;
  width: 4px;
  height: 100%;
  background: #ccc;
  left: 50%;
  transform: translateX(-50%);
  z-index: 0;
  box-shadow: inset 0 0 4px rgba(0,0,0,0.2);
}

.book-leaf {
  position: absolute;
  width: 50%;
  height: 100%;
  right: 0;
  transform-origin: left center;
  transform-style: preserve-3d;
  transition: transform 0.8s cubic-bezier(0.4, 0.0, 0.2, 1);
  cursor: pointer;
}

.book-leaf.flipped {
  transform: rotateY(-180deg);
}

.leaf-face {
  position: absolute;
  width: 100%;
  height: 100%;
  backface-visibility: hidden;
  overflow: hidden;
  background: white;
  box-shadow: inset 0 0 10px rgba(0,0,0,0.05);
}

.leaf-front {
  transform: rotateY(0deg);
  border-radius: 0 8px 8px 0;
}

.leaf-back {
  transform: rotateY(180deg);
  border-radius: 8px 0 0 8px;
}

.flipbook-controls {
  display: flex;
  align-items: center;
  gap: 20px;
  margin-top: 30px;
}

.page-indicator {
  font-weight: 600;
  color: var(--text-muted);
}

/* Common Layout CSS copied from ArrangeStep */
.page {
  width: 100%;
  height: 100%;
  position: relative;
  background: white;
  overflow: hidden;
}

.collage-slot {
  position: absolute;
  border: 4px solid white;
  background: #f0f0f0;
  overflow: hidden;
  display: flex;
  align-items: center;
  justify-content: center;
}

.collage-slot img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.layout-single .collage-slot { inset: 16px; }
.layout-split2 .collage-slot:nth-child(1) { top: 16px; bottom: 50%; left: 16px; right: 16px; border-bottom-width: 2px; }
.layout-split2 .collage-slot:nth-child(2) { top: 50%; bottom: 16px; left: 16px; right: 16px; border-top-width: 2px; }
.layout-grid3 .collage-slot:nth-child(1) { top: 16px; bottom: 50%; left: 16px; right: 16px; border-bottom-width: 2px; }
.layout-grid3 .collage-slot:nth-child(2) { top: 50%; bottom: 16px; left: 16px; right: 50%; border-top-width: 2px; border-right-width: 2px; }
.layout-grid3 .collage-slot:nth-child(3) { top: 50%; bottom: 16px; left: 50%; right: 16px; border-top-width: 2px; border-left-width: 2px; }
.layout-grid4 .collage-slot:nth-child(1) { top: 16px; bottom: 50%; left: 16px; right: 50%; border-bottom-width: 2px; border-right-width: 2px; }
.layout-grid4 .collage-slot:nth-child(2) { top: 16px; bottom: 50%; left: 50%; right: 16px; border-bottom-width: 2px; border-left-width: 2px; }
.layout-grid4 .collage-slot:nth-child(3) { top: 50%; bottom: 16px; left: 16px; right: 50%; border-top-width: 2px; border-right-width: 2px; }
.layout-grid4 .collage-slot:nth-child(4) { top: 50%; bottom: 16px; left: 50%; right: 16px; border-top-width: 2px; border-left-width: 2px; }

.full-spread-bg {
  width: 100%;
  height: 100%;
  background-size: 200% 100%;
  background-repeat: no-repeat;
}

.cover-spread {
  width: 100%;
  height: 100%;
  position: relative;
  background-size: cover;
  background-position: center;
  display: flex;
  align-items: center;
  justify-content: center;
}

.spread-overlay {
  position: absolute;
  inset: 0;
  background: linear-gradient(to top, rgba(0,0,0,0.6) 0%, transparent 60%);
  z-index: 1;
}

.cover-content {
  position: relative;
  z-index: 2;
  text-align: center;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: flex-end;
  height: 100%;
  padding-bottom: 40px;
}

.cover-title {
  color: white;
  font-size: 2.2rem;
  font-weight: 700;
  text-shadow: 0 2px 10px rgba(0,0,0,0.5);
  margin-bottom: 8px;
  line-height: 1.2;
}

.cover-subtitle {
  color: rgba(255,255,255,0.9);
  font-size: 0.9rem;
  letter-spacing: 2px;
  text-transform: uppercase;
}

.back-cover-slots {
  position: absolute;
  inset: 0;
  display: flex;
  flex-direction: row;
}

.back-cover-slots .collage-slot {
  flex: 1;
  border-radius: 0;
  border: none;
  position: relative;
  inset: 0;
}
.back-cover-slots .collage-slot:first-child {
  border-right: 2px solid white;
}

.full-width {
  width: 100%;
}
.btn-order {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 10px;
  padding: 16px;
  font-size: 1.05rem;
  border-radius: 50px;
}

.btn-text {
  display: block;
  width: 100%;
  background: none;
  border: none;
  color: var(--text-muted);
  margin-top: 16px;
  font-weight: 600;
  cursor: pointer;
  text-align: center;
}

.btn-text:hover {
  color: var(--primary);
}

.success-card {
  background: white;
  border-radius: var(--radius-lg);
  padding: 60px 40px;
  text-align: center;
  border: 1px solid var(--border-color);
  box-shadow: 0 10px 40px rgba(0,0,0,0.06);
  max-width: 540px;
  margin: 40px auto;
}

.success-icon {
  font-size: 4rem;
  margin-bottom: 20px;
}

.success-card h2 {
  font-size: 2.2rem;
  margin-bottom: 12px;
  color: var(--text-main);
}

.success-card p {
  color: var(--text-muted);
  font-size: 1.05rem;
  margin-bottom: 24px;
}

.order-ref {
  display: inline-block;
  background: rgba(127, 176, 105, 0.12);
  color: var(--primary-hover);
  padding: 8px 18px;
  border-radius: 50px;
  font-weight: 600;
  font-size: 0.95rem;
  margin-bottom: 30px;
}

@media (max-width: 968px) {
  .summary-layout {
    grid-template-columns: 1fr;
  }
}
</style>

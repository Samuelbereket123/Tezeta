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

      <div class="summary-layout">
        <!-- Book Details -->
        <div class="details-panel">
          <div class="summary-card">
            <h3>Order Summary</h3>
            <div class="summary-row">
              <span>Format</span>
              <strong>Premium Hardcover Book</strong>
            </div>
            <div class="summary-row">
              <span>Book Size</span>
              <span>8.5" x 11" (Portrait)</span>
            </div>
            <div class="summary-row">
              <span>Total Pages</span>
              <span>{{ photos.length > 0 ? photos.length : 24 }} Pages</span>
            </div>
            <div class="summary-row">
              <span>Paper Quality</span>
              <span>Eggshell Textured, 140gsm</span>
            </div>
            <div class="summary-row">
              <span>Binding</span>
              <span>Layflat Panoramic Binding</span>
            </div>
            <div class="divider"></div>
            <div class="summary-row total">
              <span>Total Price</span>
              <span class="price-val">$48.00 <small>(Free Shipping)</small></span>
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
              <div class="form-row split">
                <input type="text" v-model="form.city" placeholder="City" required />
                <input type="text" v-model="form.zip" placeholder="Postal Code" required />
              </div>
            </form>
          </div>
        </div>

        <!-- Preview Card -->
        <div class="preview-panel">
           <div class="sticky-preview">
             <div class="final-book-preview" :style="getCoverStyle()">
               <div class="cover-overlay"></div>
               <div class="preview-title">{{ cover.title || 'Your Story' }}</div>
             </div>
             <button class="btn-primary full-width btn-order" @click="completeOrder">
               <span>Place Order & Pay ($48.00)</span>
               <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round" stroke-linejoin="round"><path d="M5 12h14"/><path d="m12 5 7 7-7 7"/></svg>
             </button>
             <button class="btn-text" @click="$emit('prev')">← Back to Design</button>
           </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const props = defineProps(['photos', 'cover']);
const emit = defineEmits(['prev', 'complete']);

const orderCompleted = ref(false);

const form = ref({
  name: '',
  email: '',
  address: '',
  city: '',
  zip: ''
});

const getCoverStyle = () => {
  if (props.cover.type === 'color') {
    return { backgroundColor: props.cover.color };
  }
  return { 
    backgroundImage: `url(${props.cover.photo || 'https://images.unsplash.com/photo-1544144433-d50aff500b91?w=600'})`,
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

.sticky-preview {
  position: sticky;
  top: 100px;
}

.final-book-preview {
  width: 100%;
  aspect-ratio: 0.8;
  border-radius: 4px 12px 12px 4px;
  box-shadow: 20px 20px 50px rgba(0,0,0,0.12);
  margin-bottom: 20px;
  display: flex;
  align-items: flex-end;
  padding: 32px;
  position: relative;
  overflow: hidden;
}

.cover-overlay {
  position: absolute;
  inset: 0;
  background: linear-gradient(to top, rgba(0,0,0,0.5) 0%, transparent 60%);
}

.preview-title {
  color: white;
  font-family: 'Outfit', sans-serif;
  font-weight: 700;
  font-size: 1.8rem;
  text-transform: uppercase;
  position: relative;
  z-index: 2;
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

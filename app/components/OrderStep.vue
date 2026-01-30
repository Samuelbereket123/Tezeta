<template>
  <div class="step-container">
    <div class="step-header">
      <h2>Final Step: Order your book</h2>
      <p>Review your book details and choose your delivery address.</p>
    </div>

    <div class="summary-layout">
      <!-- Book Details -->
      <div class="details-panel">
        <div class="summary-card">
          <h3>Order Summary</h3>
          <div class="summary-row">
            <span>Product</span>
            <span>Premium Hardcover Photo Book</span>
          </div>
          <div class="summary-row">
            <span>Size</span>
            <span>8.5" x 11" (Portrait)</span>
          </div>
          <div class="summary-row">
            <span>Pages</span>
            <span>{{ photos.length }} Pages</span>
          </div>
          <div class="summary-row">
            <span>Paper</span>
            <span>Eggshell Textured, 140gsm</span>
          </div>
          <div class="divider"></div>
          <div class="summary-row total">
            <span>Total</span>
            <span>$48.00</span>
          </div>
        </div>

        <div class="delivery-card">
          <h3>Delivery Information</h3>
          <form class="delivery-form">
            <div class="form-row">
              <input type="text" placeholder="Full Name" />
            </div>
            <div class="form-row">
              <input type="text" placeholder="Address line 1" />
            </div>
            <div class="form-row split">
              <input type="text" placeholder="City" />
              <input type="text" placeholder="Postal Code" />
            </div>
          </form>
        </div>
      </div>

      <!-- Preview Card -->
      <div class="preview-panel">
         <div class="sticky-preview">
           <div class="final-book-preview" :style="getCoverStyle()">
             <div class="preview-title">{{ cover.title || 'Your Book' }}</div>
           </div>
           <button class="btn-primary full-width" @click="completeOrder">Place Order & Pay</button>
           <button class="btn-text" @click="$emit('prev')">Edit Design</button>
         </div>
      </div>
    </div>
  </div>
</template>

<script setup>
const props = defineProps(['photos', 'cover']);
const emit = defineEmits(['prev', 'complete']);

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

const completeOrder = () => {
  alert('Thank you for your order! Your memories are being crafted.');
  emit('complete');
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

.summary-layout {
  display: grid;
  grid-template-columns: 1fr 380px;
  gap: 60px;
}

.summary-card, .delivery-card {
  background: white;
  padding: 32px;
  border-radius: var(--radius-lg);
  border: 1px solid var(--border-color);
  margin-bottom: 24px;
}

h3 {
  margin-bottom: 24px;
  font-size: 1.25rem;
  color: var(--text-main);
}

.summary-row {
  display: flex;
  justify-content: space-between;
  margin-bottom: 12px;
  font-size: 1rem;
  color: var(--text-muted);
}

.summary-row.total {
  color: var(--text-main);
  font-weight: 700;
  font-size: 1.2rem;
  margin-top: 16px;
}

.divider {
  height: 1px;
  background: var(--border-color);
  margin: 16px 0;
}

.delivery-form {
  display: flex;
  flex-direction: column;
  gap: 16px;
}

.form-row input {
  width: 100%;
  padding: 12px;
  border: 1px solid var(--border-color);
  border-radius: var(--radius-sm);
  font-family: inherit;
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
  box-shadow: 20px 20px 60px rgba(0,0,0,0.1);
  margin-bottom: 24px;
  display: flex;
  align-items: flex-end;
  padding: 32px;
}

.preview-title {
  color: white;
  font-family: 'Outfit', sans-serif;
  font-weight: 700;
  font-size: 1.8rem;
  text-transform: uppercase;
}

.full-width {
  width: 100%;
}

.btn-text {
  display: block;
  width: 100%;
  background: none;
  color: var(--text-muted);
  margin-top: 16px;
  font-weight: 600;
}

.btn-text:hover {
  color: var(--primary);
}

@media (max-width: 968px) {
  .summary-layout {
    grid-template-columns: 1fr;
  }
}
</style>

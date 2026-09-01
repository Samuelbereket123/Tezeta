<template>
  <section class="testimonials">
    <div class="container">
      <div class="section-header">
        <span class="subtitle">Community Stories</span>
        <h2>What our <span class="accent">creators</span> say</h2>
      </div>
      
      <div class="testimonials-grid">
        <div v-for="(t, index) in testimonials" :key="index" class="testimonial-card">
          <div class="stars">★★★★★</div>
          <p class="content">“{{ t.content }}”</p>
          <div class="user">
            <div class="avatar" :style="{ backgroundImage: `url(${t.avatar})` }"></div>
            <div class="user-meta">
              <strong>{{ t.name }}</strong>
              <span>{{ t.location }}</span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
const testimonials = [
  {
    name: 'Sarah Jenkins',
    location: 'London, UK',
    content: 'The quality of the paper and the simplicity of the app is unmatched. I made a book of our wedding in 10 minutes.',
    avatar: 'https://images.unsplash.com/photo-1534528741775-53994a69daeb?w=150&auto=format&fit=crop&q=80'
  },
  {
    name: 'Marcus Chen',
    location: 'San Francisco, CA',
    content: 'I love the minimal design. It lets my photos speak for themselves without distracting layouts.',
    avatar: 'https://images.unsplash.com/photo-1507003211169-0a1dd7228f2d?w=150&auto=format&fit=crop&q=80'
  },
  {
    name: 'Elena Rodriguez',
    location: 'Madrid, Spain',
    content: 'Finally, a photo book service that doesn’t feel like I’m using complex design software. Beautiful results.',
    avatar: 'https://images.unsplash.com/photo-1517841905240-472988babdf9?w=150&auto=format&fit=crop&q=80'
  }
];
</script>

<style scoped>
.testimonials {
  padding: 100px 0;
  background: var(--bg-color);
}

.section-header {
  text-align: center;
  margin-bottom: 56px;
}

.subtitle {
  color: var(--primary);
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 0.08em;
  font-size: 0.82rem;
  display: block;
  margin-bottom: 8px;
}

h2 {
  font-size: clamp(2.4rem, 4vw, 3.2rem);
  letter-spacing: -0.035em;
}

.accent {
  color: var(--primary);
}

.testimonials-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 28px;
}

.testimonial-card {
  background: white;
  padding: 36px 32px;
  border-radius: var(--radius-lg);
  box-shadow: var(--shadow-sm);
  position: relative;
  transition: var(--transition);
  border: 1px solid var(--border-color);
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

.testimonial-card:hover {
  transform: translateY(-4px);
  box-shadow: var(--shadow-md);
  border-color: rgba(0, 0, 0, 0.1);
}

.stars {
  color: #f4a261;
  font-size: 1.1rem;
  letter-spacing: 2px;
  margin-bottom: 16px;
}

.content {
  font-size: 1.05rem;
  color: var(--text-main);
  line-height: 1.6;
  margin-bottom: 24px;
  font-weight: 400;
  letter-spacing: -0.01em;
}

.user {
  display: flex;
  align-items: center;
  gap: 14px;
}

.avatar {
  width: 44px;
  height: 44px;
  border-radius: 50%;
  background-size: cover;
  background-position: center;
  border: 1px solid var(--border-subtle);
}

.user-meta {
  display: flex;
  flex-direction: column;
}

.user-meta strong {
  font-size: 0.95rem;
  font-weight: 600;
  color: var(--text-main);
}

.user-meta span {
  font-size: 0.82rem;
  color: var(--text-muted);
}

@media (max-width: 968px) {
  .testimonials-grid {
    grid-template-columns: 1fr;
    max-width: 520px;
    margin: 0 auto;
  }
}
</style>

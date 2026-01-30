<template>
  <section class="testimonials">
    <div class="container">
      <div class="section-header">
        <span class="subtitle">Community Love</span>
        <h2>What our <span class="accent">customers</span> say</h2>
      </div>
      
      <div class="testimonials-grid">
        <div v-for="(t, index) in testimonials" :key="index" class="testimonial-card">
          <div class="quote-icon">"</div>
          <p class="content">{{ t.content }}</p>
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
    avatar: 'https://i.pravatar.cc/150?u=sarah'
  },
  {
    name: 'Marcus Chen',
    location: 'San Francisco, CA',
    content: 'I love the minimal design. It lets my photos speak for themselves without distracting layouts.',
    avatar: 'https://i.pravatar.cc/150?u=marcus'
  },
  {
    name: 'Elena Rodriguez',
    location: 'Madrid, Spain',
    content: 'Finally, a photo book service that doesn’t feel like I’m using complex design software. Beautiful results.',
    avatar: 'https://i.pravatar.cc/150?u=elena'
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
  margin-bottom: 60px;
}

.subtitle {
  color: var(--primary);
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 2px;
  font-size: 0.9rem;
}

h2 {
  font-size: 3rem;
  margin-top: 10px;
}

.accent {
  color: var(--primary);
}

.testimonials-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 30px;
}

.testimonial-card {
  background: white;
  padding: 40px;
  border-radius: var(--radius-lg);
  box-shadow: 0 10px 40px rgba(0,0,0,0.03);
  position: relative;
  transition: transform 0.3s ease;
  border: 1px solid var(--border-color);
}

.testimonial-card:hover {
  transform: translateY(-10px);
}

.quote-icon {
  font-family: 'Playfair Display', serif;
  font-size: 5rem;
  color: var(--primary);
  opacity: 0.2;
  position: absolute;
  top: 10px;
  left: 30px;
  line-height: 1;
}

.content {
  font-size: 1.1rem;
  color: var(--text-muted);
  font-style: italic;
  margin-bottom: 30px;
  position: relative;
  z-index: 1;
}

.user {
  display: flex;
  align-items: center;
  gap: 15px;
}

.avatar {
  width: 50px;
  height: 50px;
  border-radius: 50%;
  background-size: cover;
  background-position: center;
  border: 2px solid white;
  box-shadow: 0 5px 15px rgba(0,0,0,0.1);
}

.user-meta {
  display: flex;
  flex-direction: column;
}

.user-meta strong {
  font-size: 1rem;
  color: var(--text-main);
}

.user-meta span {
  font-size: 0.85rem;
  color: var(--text-muted);
}

@media (max-width: 968px) {
  .testimonials-grid {
    grid-template-columns: 1fr;
    max-width: 500px;
    margin: 0 auto;
  }
}
</style>

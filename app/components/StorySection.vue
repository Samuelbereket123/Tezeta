<template>
  <section class="story">
    <div class="container story-content">
      <div class="story-image-wrapper">
        <div class="story-image">
          <img src="https://images.unsplash.com/photo-1516627145497-ae6968895b74?q=80&w=2040&auto=format&fit=crop" alt="Founder Story" />
          <div class="image-overlay"></div>
        </div>
        <div class="experience-badge">
          <span class="years">10+</span>
          <span class="text">Years of Memory Making</span>
        </div>
      </div>
      
      <div class="story-text">
        <span class="subtitle">Our Story</span>
        <h2>It started with a box of <span class="accent">old polaroids</span>.</h2>
        <p class="lead">Tizeta means "Memory" in Ethiopia. We believe that every photo tells a story that deserves a home in the physical world.</p>
        <div class="story-details">
          <p>Back in 2016, our founder found a dusty box of fading family photos. They were precious, but they were forgotten in a closet. We realized that in our digital age, our most important moments are trapped on our phones, rarely looked at again.</p>
          <p>We built Tizeta-Pages to change that. No complex design tools, no steep learning curves—just a simple, beautiful way to rescue your memories and hold them in your hands.</p>
        </div>
        <div class="founder-quote">
          <p>"A photo book isn't just paper and ink; it's a bridge to who we were and those we love."</p>
          <div class="founder-info">
            <strong>Samuel Abera</strong>
            <span>Founder of Tizeta-Pages</span>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
.story {
  padding: 120px 0;
  overflow: hidden;
}

.story-content {
  display: grid;
  grid-template-columns: 1fr 1.2fr;
  align-items: center;
  gap: 80px;
}

.story-image-wrapper {
  position: relative;
}

.story-image {
  position: relative;
  border-radius: var(--radius-lg);
  overflow: hidden;
  box-shadow: 20px 20px 60px rgba(0,0,0,0.08);
}

.story-image img {
  width: 100%;
  height: 600px;
  object-fit: cover;
  transition: transform 0.8s ease;
}

.story-image:hover img {
  transform: scale(1.05);
}

.image-overlay {
  position: absolute;
  inset: 0;
  background: linear-gradient(to top, rgba(0,0,0,0.2), transparent);
}

.experience-badge {
  position: absolute;
  bottom: -30px;
  right: -30px;
  background: var(--primary);
  color: white;
  padding: 30px;
  border-radius: var(--radius-md);
  box-shadow: 10px 10px 30px rgba(127, 176, 105, 0.3);
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
}

.years {
  font-size: 2.5rem;
  font-weight: 800;
  line-height: 1;
  font-family: 'Outfit', sans-serif;
}

.experience-badge .text {
  font-size: 0.8rem;
  font-weight: 500;
  opacity: 0.9;
  max-width: 80px;
}

.subtitle {
  color: var(--primary);
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 2px;
  font-size: 0.9rem;
  margin-bottom: 16px;
  display: block;
}

h2 {
  font-size: 3.5rem;
  line-height: 1.1;
  margin-bottom: 30px;
  letter-spacing: -1.5px;
}

.accent {
  color: var(--primary);
}

.lead {
  font-size: 1.4rem;
  color: var(--text-main);
  font-weight: 500;
  margin-bottom: 24px;
  line-height: 1.4;
}

.story-details p {
  color: var(--text-muted);
  font-size: 1.1rem;
  margin-bottom: 20px;
}

.founder-quote {
  margin-top: 40px;
  padding-left: 24px;
  border-left: 4px solid var(--primary);
  font-style: italic;
}

.founder-quote p {
  font-size: 1.25rem;
  color: var(--text-main);
  margin-bottom: 12px;
}

.founder-info {
  display: flex;
  flex-direction: column;
  font-style: normal;
}

.founder-info strong {
  font-size: 1rem;
  color: var(--text-main);
}

.founder-info span {
  font-size: 0.85rem;
  color: var(--text-muted);
}

@media (max-width: 1024px) {
  .story-content {
    grid-template-columns: 1fr;
    gap: 60px;
  }
  .story-image img {
    height: 400px;
  }
  h2 {
    font-size: 2.5rem;
  }
}
</style>

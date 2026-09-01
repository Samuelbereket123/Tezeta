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
  padding: 100px 0;
  overflow: hidden;
}

.story-content {
  display: grid;
  grid-template-columns: 1fr 1.15fr;
  align-items: center;
  gap: 60px;
}

.story-image-wrapper {
  position: relative;
}

.story-image {
  position: relative;
  border-radius: var(--radius-lg);
  overflow: hidden;
  box-shadow: var(--shadow-lg);
  border: 1px solid var(--border-subtle);
}

.story-image img {
  width: 100%;
  height: 520px;
  object-fit: cover;
  transition: transform 0.6s ease;
}

.story-image:hover img {
  transform: scale(1.03);
}

.image-overlay {
  position: absolute;
  inset: 0;
  background: linear-gradient(to top, rgba(0,0,0,0.25), transparent 60%);
}

.experience-badge {
  position: absolute;
  bottom: -20px;
  right: -20px;
  background: var(--text-main);
  color: white;
  padding: 22px 26px;
  border-radius: var(--radius-md);
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.18);
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
  border: 1px solid rgba(255, 255, 255, 0.1);
}

.years {
  font-size: 2.2rem;
  font-weight: 800;
  line-height: 1;
  font-family: var(--font-display);
  letter-spacing: -0.03em;
  color: #ffffff;
}

.experience-badge .text {
  font-size: 0.78rem;
  font-weight: 500;
  opacity: 0.85;
  max-width: 90px;
  margin-top: 4px;
}

.subtitle {
  color: var(--primary);
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 0.08em;
  font-size: 0.82rem;
  margin-bottom: 12px;
  display: block;
}

h2 {
  font-size: clamp(2.4rem, 4vw, 3.2rem);
  line-height: 1.12;
  margin-bottom: 24px;
  letter-spacing: -0.035em;
}

.accent {
  color: var(--primary);
}

.lead {
  font-size: 1.25rem;
  color: var(--text-main);
  font-weight: 500;
  margin-bottom: 20px;
  line-height: 1.5;
  letter-spacing: -0.01em;
}

.story-details p {
  color: var(--text-muted);
  font-size: 1.02rem;
  margin-bottom: 16px;
  line-height: 1.6;
}

.founder-quote {
  margin-top: 32px;
  padding-left: 20px;
  border-left: 3px solid var(--primary);
  font-style: normal;
}

.founder-quote p {
  font-size: 1.15rem;
  color: var(--text-main);
  margin-bottom: 10px;
  font-weight: 500;
  line-height: 1.45;
  letter-spacing: -0.01em;
}

.founder-info {
  display: flex;
  flex-direction: column;
}

.founder-info strong {
  font-size: 0.95rem;
  color: var(--text-main);
}

.founder-info span {
  font-size: 0.82rem;
  color: var(--text-muted);
}

@media (max-width: 1024px) {
  .story-content {
    grid-template-columns: 1fr;
    gap: 50px;
  }
  .story-image img {
    height: 380px;
  }
  .experience-badge {
    bottom: -15px;
    right: 10px;
  }
}
</style>

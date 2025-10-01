<template>
  <div class="home-container">
    <!-- Navigation -->
    <nav class="navbar">
      <div class="nav-content">
        <div class="logo">
          <span class="logo-text">Thin</span>
          <span class="logo-accent">Ice</span>
        </div>
        <ul class="nav-links">
          <li><a href="#about">About</a></li>
          <li><a href="#projects">Projects</a></li>
          <li><a href="#publications">Publications</a></li>
          <li><a href="#team">Team</a></li>
          <li><a href="#contact" class="contact-btn">Contact</a></li>
        </ul>
      </div>
    </nav>

    <!-- Hero Section -->
    <section class="hero">
      <div class="floating-shape shape-1"></div>
      <div class="floating-shape shape-2"></div>
      <div class="hero-content">
        <h1 class="hero-title">
          <span class="title-line-1">National Science Foundation</span>
          <span class="title-line-2">Thin Ice</span>
        </h1>
        <p class="hero-subtitle">
          Exploring the frontiers of scientific research
        </p>
        <div class="hero-actions">
          <button class="btn-primary" @click="scrollToSection('projects')">
            View Our Work
          </button>
          <button class="btn-secondary" @click="scrollToSection('publications')">
            Recent Publications
          </button>
        </div>
      </div>
    </section>

    <!-- Stats Section -->
    <section class="stats" ref="statsSection">
      <div class="stats-grid">
        <div 
          v-for="(stat, index) in stats" 
          :key="index"
          class="stat-card"
          :class="{ 'animate-in': statsVisible }"
          :style="{ animationDelay: `${index * 0.15}s` }"
        >
          <div class="stat-number">{{ stat.value }}</div>
          <div class="stat-label">{{ stat.label }}</div>
        </div>
      </div>
    </section>

    <!-- Featured Research -->
    <section class="featured" ref="featuredSection">
      <div class="section-header">
        <h2 class="section-title">Featured Research</h2>
        <p class="section-subtitle">Our latest breakthrough discoveries and ongoing investigations</p>
      </div>
      
      <div class="research-grid">
        <article 
          v-for="(item, index) in featuredResearch" 
          :key="index"
          class="research-card"
          :class="{ 'animate-in': cardsVisible }"
          :style="{ animationDelay: `${index * 0.2}s` }"
          @mouseenter="cardHover = index"
          @mouseleave="cardHover = null"
        >
          <div class="card-accent-bar"></div>
          <div class="card-icon">
            <div class="icon-circle">{{ item.icon }}</div>
          </div>
          <h3 class="card-title">{{ item.title }}</h3>
          <p class="card-description">{{ item.description }}</p>
          <a href="#" class="card-link">
            Learn more 
            <span class="arrow">→</span>
          </a>
        </article>
      </div>
    </section>

    <!-- Call to Action -->
    <section class="cta">
      <div class="cta-pattern"></div>
      <div class="cta-content">
        <h2 class="cta-title">Join Our Research Community</h2>
        <p class="cta-text">
          Collaborate with leading researchers and contribute to groundbreaking discoveries
        </p>
        <button class="btn-cta" @click="handleCTA">Get Started</button>
      </div>
    </section>

    <!-- Footer -->
    <footer class="footer">
      <div class="footer-content">
        <p>&copy; 2024 Research Lab. All rights reserved.</p>
        <div class="footer-links">
          <a href="#">Privacy</a>
          <a href="#">Terms</a>
          <a href="#">Accessibility</a>
        </div>
      </div>
    </footer>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue'

// Types
interface Stat {
  value: string
  label: string
}

interface ResearchItem {
  icon: string
  title: string
  description: string
}

// Reactive data
const cardHover = ref<number | null>(null)
const statsVisible = ref(false)
const cardsVisible = ref(false)
const statsSection = ref<HTMLElement>()
const featuredSection = ref<HTMLElement>()

const stats = ref<Stat[]>([
  { value: '150+', label: 'Research Papers' },
  { value: '45', label: 'Active Projects' },
  { value: '28', label: 'Team Members' },
  { value: '12', label: 'Partner Institutions' }
])

const featuredResearch = ref<ResearchItem[]>([
  {
    icon: '🧬',
    title: 'Genomic Analysis',
    description: 'Investigating genetic markers and their role in disease prevention through advanced sequencing techniques.'
  },
  {
    icon: '🤖',
    title: 'Machine Learning Applications',
    description: 'Developing novel algorithms for pattern recognition in complex biological systems and medical imaging.'
  },
  {
    icon: '🌍',
    title: 'Climate Impact Studies',
    description: 'Analyzing environmental factors and their long-term effects on ecosystem dynamics and biodiversity.'
  }
])

// Methods
const scrollToSection = (sectionId: string) => {
  const element = document.getElementById(sectionId)
  if (element) {
    element.scrollIntoView({ behavior: 'smooth' })
  }
}

const handleCTA = () => {
  console.log('CTA clicked')
}

// Intersection Observer
let observer: IntersectionObserver

onMounted(() => {
  const options = {
    threshold: 0.2,
    rootMargin: '0px'
  }
  
  observer = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
      if (entry.isIntersecting) {
        if (entry.target === statsSection.value) {
          statsVisible.value = true
        }
        if (entry.target === featuredSection.value) {
          cardsVisible.value = true
        }
      }
    })
  }, options)
  
  if (statsSection.value) observer.observe(statsSection.value)
  if (featuredSection.value) observer.observe(featuredSection.value)
})

onUnmounted(() => {
  if (observer) {
    observer.disconnect()
  }
})
</script>

<style scoped>
/* Base Styles */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.home-container {
  min-height: 100vh;
  background: #FFFFFF;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  color: #2C3E50;
  overflow-x: hidden;
}

/* Navigation */
.navbar {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  background: rgba(255, 255, 255, 0.98);
  backdrop-filter: blur(10px);
  z-index: 1000;
  padding: 1.5rem 0;
  border-bottom: 1px solid #F0F0F0;
  animation: slideDown 0.6s ease-out;
}

@keyframes slideDown {
  from {
    transform: translateY(-100%);
    opacity: 0;
  }
  to {
    transform: translateY(0);
    opacity: 1;
  }
}

.nav-content {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 2rem;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.logo {
  font-size: 1.5rem;
  font-weight: 700;
}

.logo-text {
  color: #2C3E50;
}

.logo-accent {
  background: linear-gradient(135deg, #B8E0D2, #95D5B2);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  margin-left: 0.25rem;
}

.nav-links {
  display: flex;
  gap: 2.5rem;
  list-style: none;
  align-items: center;
}

.nav-links a {
  color: #6C757D;
  text-decoration: none;
  font-weight: 500;
  position: relative;
  transition: color 0.3s ease;
}

.nav-links a::after {
  content: '';
  position: absolute;
  bottom: -5px;
  left: 0;
  width: 0;
  height: 2px;
  background: #B8E0D2;
  transition: width 0.3s ease;
}

.nav-links a:hover::after {
  width: 100%;
}

.nav-links a:hover {
  color: #2C3E50;
}

.contact-btn {
  background: linear-gradient(135deg, #B8E0D2, #95D5B2);
  color: #2C3E50 !important;
  padding: 0.6rem 1.8rem;
  border-radius: 25px;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.contact-btn::after {
  display: none;
}

.contact-btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 5px 15px rgba(149, 213, 178, 0.3);
}

/* Hero Section */
.hero {
  margin-top: 80px;
  padding: 8rem 2rem;
  min-height: 80vh;
  display: flex;
  align-items: center;
  justify-content: center;
  background: linear-gradient(135deg, #FAFAFA 0%, #F5F5F5 100%);
  position: relative;
}

.floating-shape {
  position: absolute;
  border-radius: 50%;
  opacity: 0.1;
  animation: float 20s infinite ease-in-out;
}

.shape-1 {
  width: 300px;
  height: 300px;
  background: linear-gradient(135deg, #B8E0D2, #95D5B2);
  top: 10%;
  left: 5%;
}

.shape-2 {
  width: 200px;
  height: 200px;
  background: linear-gradient(135deg, #EAC4D5, #F4A6C9);
  bottom: 20%;
  right: 10%;
  animation-delay: -10s;
}

@keyframes float {
  0%, 100% { transform: translate(0, 0) rotate(0deg); }
  33% { transform: translate(30px, -30px) rotate(120deg); }
  66% { transform: translate(-20px, 20px) rotate(240deg); }
}

.hero-content {
  max-width: 900px;
  text-align: center;
  position: relative;
  z-index: 1;
}

.hero-title {
  font-size: clamp(2.5rem, 5vw, 4rem);
  font-weight: 700;
  line-height: 1.2;
  margin-bottom: 2rem;
}

.title-line-1 {
  display: block;
  animation: fadeInUp 0.8s ease-out 0.2s both;
}

.title-line-2 {
  display: block;
  background: linear-gradient(135deg, #B8E0D2, #EAC4D5);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  animation: fadeInUp 0.8s ease-out 0.4s both;
}

@keyframes fadeInUp {
  from {
    opacity: 0;
    transform: translateY(30px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.hero-subtitle {
  font-size: 1.25rem;
  color: #6C757D;
  margin-bottom: 3rem;
  line-height: 1.6;
  max-width: 600px;
  margin-left: auto;
  margin-right: auto;
  animation: fadeInUp 0.8s ease-out 0.6s both;
}

.hero-actions {
  display: flex;
  gap: 1.5rem;
  justify-content: center;
  animation: fadeInUp 0.8s ease-out 0.8s both;
}

.btn-primary, .btn-secondary {
  padding: 1rem 2.5rem;
  font-size: 1rem;
  font-weight: 600;
  border: none;
  border-radius: 25px;
  cursor: pointer;
  transition: all 0.3s ease;
}

.btn-primary {
  background: linear-gradient(135deg, #B8E0D2, #95D5B2);
  color: #2C3E50;
}

.btn-primary:hover {
  transform: translateY(-3px);
  box-shadow: 0 10px 25px rgba(149, 213, 178, 0.3);
}

.btn-secondary {
  background: transparent;
  color: #2C3E50;
  border: 2px solid #EAC4D5;
}

.btn-secondary:hover {
  background: linear-gradient(135deg, #EAC4D5, #F4A6C9);
  border-color: transparent;
  transform: translateY(-3px);
  box-shadow: 0 10px 25px rgba(234, 196, 213, 0.3);
}

/* Stats Section */
.stats {
  padding: 5rem 2rem;
  background: #FFFFFF;
}

.stats-grid {
  max-width: 1000px;
  margin: 0 auto;
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 3rem;
}

.stat-card {
  text-align: center;
  opacity: 0;
  transform: translateY(20px);
}

.stat-card.animate-in {
  animation: scaleIn 0.6s ease-out forwards;
}

@keyframes scaleIn {
  to {
    opacity: 1;
    transform: translateY(0) scale(1);
  }
}

.stat-number {
  font-size: 3rem;
  font-weight: 700;
  background: linear-gradient(135deg, #B8E0D2, #95D5B2);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  margin-bottom: 0.5rem;
}

.stat-label {
  color: #6C757D;
  font-size: 1.1rem;
}

/* Featured Research */
.featured {
  padding: 5rem 2rem;
  background: #F8F9FA;
}

.section-header {
  text-align: center;
  margin-bottom: 4rem;
}

.section-title {
  font-size: 2.5rem;
  font-weight: 700;
  color: #2C3E50;
  margin-bottom: 1rem;
}

.section-subtitle {
  color: #6C757D;
  font-size: 1.1rem;
}

.research-grid {
  max-width: 1200px;
  margin: 0 auto;
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 2.5rem;
}

.research-card {
  background: #FFFFFF;
  padding: 2.5rem;
  border-radius: 12px;
  position: relative;
  overflow: hidden;
  box-shadow: 0 2px 15px rgba(0, 0, 0, 0.05);
  transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
  opacity: 0;
  transform: translateY(30px);
}

.research-card.animate-in {
  animation: cardSlideIn 0.6s ease-out forwards;
}

@keyframes cardSlideIn {
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.card-accent-bar {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 4px;
  background: linear-gradient(90deg, #B8E0D2, #EAC4D5);
  transform: scaleX(0);
  transition: transform 0.3s ease;
}

.research-card:hover .card-accent-bar {
  transform: scaleX(1);
}

.research-card:hover {
  transform: translateY(-8px);
  box-shadow: 0 15px 40px rgba(0, 0, 0, 0.1);
}

.card-icon {
  margin-bottom: 1.5rem;
}

.icon-circle {
  width: 60px;
  height: 60px;
  background: linear-gradient(135deg, #B8E0D2, #EAC4D5);
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 1.5rem;
  transition: transform 0.3s ease;
}

.research-card:hover .icon-circle {
  transform: scale(1.1) rotate(5deg);
}

.card-title {
  font-size: 1.5rem;
  font-weight: 600;
  margin-bottom: 1rem;
  color: #2C3E50;
}

.card-description {
  color: #6C757D;
  line-height: 1.6;
  margin-bottom: 1.5rem;
}

.card-link {
  color: #B8E0D2;
  text-decoration: none;
  font-weight: 600;
  display: inline-flex;
  align-items: center;
  gap: 0.5rem;
  transition: gap 0.3s ease;
}

.card-link:hover {
  gap: 1rem;
}

.card-link .arrow {
  transition: transform 0.3s ease;
}

.card-link:hover .arrow {
  transform: translateX(3px);
}

/* CTA Section */
.cta {
  padding: 6rem 2rem;
  background: linear-gradient(135deg, #B8E0D2, #EAC4D5);
  position: relative;
  overflow: hidden;
}

.cta-pattern {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  opacity: 0.1;
  background-image: 
    repeating-linear-gradient(45deg, transparent, transparent 35px, rgba(255,255,255,.1) 35px, rgba(255,255,255,.1) 70px);
}

.cta-content {
  max-width: 600px;
  margin: 0 auto;
  text-align: center;
  position: relative;
  z-index: 1;
}

.cta-title {
  font-size: 2.5rem;
  font-weight: 700;
  color: #FFFFFF;
  margin-bottom: 1rem;
}

.cta-text {
  color: #FFFFFF;
  font-size: 1.2rem;
  margin-bottom: 2rem;
  opacity: 0.95;
}

.btn-cta {
  background: #FFFFFF;
  color: #2C3E50;
  padding: 1rem 3rem;
  font-size: 1.1rem;
  font-weight: 600;
  border: none;
  border-radius: 25px;
  cursor: pointer;
  transition: all 0.3s ease;
}

.btn-cta:hover {
  transform: translateY(-3px);
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
}

/* Footer */
.footer {
  padding: 3rem 2rem;
  background: #FFFFFF;
  border-top: 1px solid #F0F0F0;
}

.footer-content {
  max-width: 1200px;
  margin: 0 auto;
  display: flex;
  justify-content: space-between;
  align-items: center;
  color: #6C757D;
}

.footer-links {
  display: flex;
  gap: 2rem;
}

.footer-links a {
  color: #6C757D;
  text-decoration: none;
  transition: color 0.3s ease;
}

.footer-links a:hover {
  color: #B8E0D2;
}

/* Responsive Design */
@media (max-width: 768px) {
  .nav-links {
    display: none;
  }
  
  .hero {
    padding: 4rem 1.5rem;
  }
  
  .hero-actions {
    flex-direction: column;
    align-items: center;
  }
  
  .btn-primary, .btn-secondary {
    width: 200px;
  }
  
  .stats-grid {
    grid-template-columns: repeat(2, 1fr);
    gap: 2rem;
  }
  
  .research-grid {
    grid-template-columns: 1fr;
  }
  
  .footer-content {
    flex-direction: column;
    gap: 1rem;
    text-align: center;
  }
  
  .floating-shape {
    display: none;
  }
}
</style>
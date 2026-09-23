<script setup lang="ts">
import { onMounted, ref } from 'vue';

interface HeroProps {
  name: string;
  role: string;
  location: string;
  headline: string;
  resumeUrl: string;
  linkedinUrl: string;
  email: string;
  currentCompany?: string;
  currentPosition?: string;
  why?: string;
  showPlot?: boolean;
}

withDefaults(defineProps<HeroProps>(), {
  currentCompany: '',
  currentPosition: '',
  why: '',
  showPlot: false,
});

const inited = ref(false);

onMounted(() => {
  requestAnimationFrame(() => {
    inited.value = true;
  });
});
</script>

<template>
  <section class="hero" id="top" :class="{ 'hero-in': inited }">
    <div class="wrap">
      <div class="hero-mast masthead">
        <p class="hero-mast-label">andres felipe orjuela arias</p>
      </div>

      <div class="hero-body">
        <div class="hero-copy">
          <h1 class="hero-name">{{ name }}</h1>
          <p class="hero-role">{{ role }}<span class="hero-sep" aria-hidden="true">·</span>{{ location }}</p>
          <p class="hero-headline">{{ headline }}</p>

          <blockquote class="hero-why" v-if="why">
            <span class="datalabel hero-why-label">why this work</span>
            <p class="hero-why-text">{{ why }}</p>
          </blockquote>

          <div class="hero-ctas">
            <a :href="resumeUrl" class="btn btn-primary" download>
              <svg width="12" height="12" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true">
                <path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4" />
                <polyline points="7 10 12 15 17 10" />
                <line x1="12" y1="15" x2="12" y2="3" />
              </svg>
              <span>Download resume</span>
            </a>
            <a :href="`mailto:${email}`" class="btn btn-secondary">Get in touch</a>
          </div>
        </div>

        <figure class="hero-plate">
          <div class="plate-frame">
            <img
              class="plate-img"
              src="/img/hero.jpg"
              alt="Andres Orjuela working from his data engineering workspace"
              width="560"
              height="640"
              loading="eager"
            />
            <div class="plate-empty" aria-hidden="true">
              <span class="plate-empty-label">plate · workspace</span>
            </div>
          </div>
          <figcaption class="plate-caption">
            <span class="datalabel">Workspace</span>
          </figcaption>
        </figure>
      </div>
    </div>
  </section>
</template>

<style scoped>
.hero {
  padding: clamp(48px, 8vw, 88px) 0 clamp(48px, 7vw, 80px);
  position: relative;
  overflow: hidden;
}

.hero::before {
  content: '';
  position: absolute;
  top: -20%;
  right: -10%;
  width: 600px;
  height: 600px;
  background: var(--accent-warm);
  opacity: 0.08;
  border-radius: 60% 40% 30% 70% / 60% 30% 70% 40%;
  animation: blobMorph 12s ease-out infinite;
  pointer-events: none;
}

@keyframes blobMorph {
  0%, 100% {
    border-radius: 60% 40% 30% 70% / 60% 30% 70% 40%;
    transform: translate(0, 0) rotate(0deg);
  }
  50% {
    border-radius: 30% 60% 70% 40% / 50% 60% 30% 60%;
    transform: translate(50px, 30px) rotate(180deg);
  }
}

.hero-mast-label {
  font-family: var(--font-mono);
  font-size: 11px;
  font-weight: 500;
  letter-spacing: 0.22em;
  text-transform: uppercase;
  color: var(--text-3);
  margin: 0;
}

.hero-body {
  display: grid;
  grid-template-columns: minmax(0, 1fr) 320px;
  gap: clamp(28px, 5vw, 64px);
  align-items: center;
}

.hero-copy {
  display: grid;
  gap: 22px;
}

.hero-name {
  font-family: var(--font-display);
  font-size: clamp(36px, 5.5vw, 64px);
  font-weight: 700;
  letter-spacing: -0.02em;
  line-height: 1.1;
  color: var(--text-0);
  margin: 0;
  text-wrap: balance;
}

.hero-role {
  font-family: var(--font-mono);
  font-size: 13px;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  color: var(--text-2);
  margin: 0;
}

.hero-sep {
  margin: 0 10px;
  color: var(--text-3);
}

.hero-headline {
  font-size: clamp(18px, 2.2vw, 23px);
  line-height: 1.55;
  color: var(--text-1);
  margin: 0;
  max-width: 52ch;
  font-weight: 500;
}

.hero-ctas {
  display: flex;
  flex-wrap: wrap;
  gap: 12px;
  margin-top: 4px;
}

.hero-why {
  margin: 0;
  padding-left: 18px;
  border-left: 2px solid var(--line-accent);
  display: grid;
  gap: 8px;
  max-width: 58ch;
}

.hero-why-text {
  margin: 0;
  font-size: clamp(14.5px, 1.6vw, 16.5px);
  line-height: 1.7;
  color: var(--text-1);
  font-weight: 500;
  text-wrap: balance;
}

.btn {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  font-family: var(--font-display);
  font-size: 14px;
  font-weight: 600;
  padding: 12px 24px;
  border-radius: var(--radius-full);
  transition: all var(--dur-fast) var(--ease-elastic);
}

.btn-primary {
  background: var(--accent-warm);
  color: var(--ink-0);
  border: none;
  box-shadow: var(--shadow-soft);
}

.btn-primary:hover {
  transform: translateY(-2px);
  box-shadow: var(--shadow-medium);
}

.btn-secondary {
  background: transparent;
  color: var(--text-0);
  border: 2px solid var(--line-strong);
}

.btn-secondary:hover {
  border-color: var(--accent-warm);
  color: var(--accent-warm);
}

/* ---- workspace plate ---- */

.hero-plate {
  margin: 0;
}

.plate-frame {
  position: relative;
  aspect-ratio: 4 / 4.6;
  overflow: hidden;
  border-radius: var(--radius-xl);
  background: var(--ink-3);
  box-shadow: var(--shadow-medium);
}

.plate-img {
  position: absolute;
  inset: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
  filter: grayscale(0.3) contrast(1.05);
  transition: transform var(--dur-slow) var(--ease-out);
}

.plate-frame:hover .plate-img {
  transform: scale(1.03);
}

.plate-empty {
  position: absolute;
  inset: 0;
  display: none;
  align-items: center;
  justify-content: center;
  border: 1px dashed var(--line-strong);
  border-radius: var(--radius-xl);
}

.plate-empty-label {
  font-family: var(--font-mono);
  font-size: 10px;
  letter-spacing: 0.14em;
  text-transform: uppercase;
  color: var(--text-3);
}

.plate-img:not([src]),
.plate-img[src=''] {
  display: none;
}

.plate-caption {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 12px;
  padding-top: 12px;
  font-size: 13px;
  color: var(--text-3);
}

@media (max-width: 760px) {
  .hero-body {
    grid-template-columns: 1fr;
  }

  .hero-plate {
    max-width: 340px;
  }
}
</style>

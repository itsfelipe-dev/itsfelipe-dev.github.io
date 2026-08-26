<script setup lang="ts">
interface GalleryImage {
  src: string;
  srcset?: string;
  caption: string;
}

defineProps<{
  images: GalleryImage[];
  videoSrc: string;
  videoPoster: string;
}>();

const facts = [
  { key: 'rank', value: '#1' },
  { key: 'country', value: 'Colombia' },
  { key: 'event', value: 'WorldSkills Lyon 2024' },
  { key: 'discipline', value: 'Cybersecurity' },
];

const coverage = [
  {
    label: 'SENA · official news',
    title: '33 aprendices representarán a Colombia en WorldSkills Francia',
    meta: 'qualification announced · Jul 2024',
    href: 'https://www.sena.edu.co/es-co/Noticias/Paginas/noticia.aspx?IdNoticia=7378',
  },
  {
    label: 'SENA · official news',
    title: 'WorldSkills: el SENA enciende la llama de los “Juegos Olímpicos” de los aprendices',
    meta: 'national selection · Jul 2024',
    href: 'https://www.sena.edu.co/es-co/Noticias/Paginas/noticia.aspx?IdNoticia=7353',
  },
  {
    label: 'Facebook · video',
    title: 'Competition coverage',
    meta: 'video',
    href: 'https://www.facebook.com/watch/?v=1551616472097032',
  },
  {
    label: 'Facebook · video',
    title: 'Competition coverage',
    meta: 'video',
    href: 'https://www.facebook.com/watch/?v=428921262940593',
  },
  {
    label: 'YouTube · livestream',
    title: 'WorldSkills Lyon 2024 - closing ceremony',
    meta: 'live stream',
    href: 'https://www.youtube.com/live/J0o8zFG9Es8?si=oRXjVsYYBMyovY6N&t=1264',
  },
];
</script>

<template>
  <div class="medal reveal reveal--up">
    <div class="medal-head">
      <div class="medal-copy">
        <p class="medal-lede">
          <strong>WorldSkills is the “Olympics of skills”</strong> - the world’s largest
          skills championship, held every two years since 1950. At the 47th edition in
          Lyon, 1,400+ competitors from ~70 countries fought across ~60 trades. This page
          earned Colombia’s seat in <strong>Cybersecurity</strong> - and returned to mentor
          the next cohort.
        </p>
        <p class="medal-sub">the full record lives on the dedicated story page</p>
      </div>

      <dl class="medal-facts">
        <div v-for="f in facts" :key="f.key" class="medal-fact">
          <dt class="datalabel">{{ f.key }}</dt>
          <dd class="medal-fact-value">{{ f.value }}</dd>
        </div>
      </dl>
    </div>

    <figure class="medal-video">
      <video
        class="medal-video-el"
        controls
        playsinline
        preload="metadata"
        :poster="videoPoster"
        :src="videoSrc"
      ></video>
      <figcaption class="medal-caption">
        <span class="datalabel">fig · the record on tape</span>
      </figcaption>
    </figure>

    <div class="medal-gallery">
      <figure v-for="img in images" :key="img.src" class="medal-shot">
        <img
          class="medal-shot-img"
          :src="img.src"
          :srcset="img.srcset"
          :alt="`WorldSkills Lyon 2024 - ${img.caption}`"
          loading="lazy"
          decoding="async"
        />
        <figcaption class="medal-caption">
          <span class="datalabel">{{ img.caption }}</span>
        </figcaption>
      </figure>
    </div>

    <details class="medal-coverage">
      <summary class="medal-coverage-summary">
        <span class="medal-coverage-label">Press & coverage</span>
        <span class="medal-coverage-hint">5 sources · expand to browse</span>
        <svg class="medal-coverage-chevron" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true">
          <polyline points="6 9 12 15 18 9" />
        </svg>
      </summary>
      <ul class="medal-coverage-list">
        <li v-for="c in coverage" :key="c.href" class="medal-coverage-item">
          <a :href="c.href" target="_blank" rel="noopener noreferrer" class="medal-coverage-link">
            <span class="medal-coverage-body">
              <span class="medal-coverage-meta">{{ c.label }}</span>
              <span class="medal-coverage-title">{{ c.title }}</span>
              <span class="medal-coverage-note">{{ c.meta }}</span>
            </span>
            <svg class="medal-coverage-ext" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true">
              <path d="M7 17 17 7M7 7h10v10" />
            </svg>
          </a>
        </li>
      </ul>
    </details>

    <a href="/worldskills" class="btn btn-secondary medal-cta">
      Open the full WorldSkills story
      <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true">
        <line x1="5" y1="12" x2="19" y2="12" />
        <polyline points="12 5 19 12 12 19" />
      </svg>
    </a>
  </div>
</template>

<style scoped>
.medal {
  margin-top: clamp(36px, 5vw, 52px);
  display: grid;
  gap: clamp(28px, 4vw, 44px);
}

.medal-head {
  display: grid;
  grid-template-columns: minmax(0, 1.3fr) minmax(0, 1fr);
  gap: clamp(24px, 4vw, 48px);
  align-items: start;
}

.medal-copy {
  display: grid;
  gap: 12px;
}

.medal-lede {
  margin: 0;
  font-size: clamp(16px, 2vw, 20px);
  line-height: 1.65;
  color: var(--text-1);
  max-width: 60ch;
}

.medal-lede strong {
  color: var(--text-0);
  font-weight: 700;
}

.medal-sub {
  margin: 0;
  font-family: var(--font-mono);
  font-size: 11px;
  letter-spacing: 0.14em;
  text-transform: uppercase;
  color: var(--text-3);
}

.medal-facts {
  margin: 0;
  padding: 0;
  list-style: none;
  border: 1px solid var(--line-strong);
  border-radius: var(--radius-lg);
  background: var(--ink-2);
  overflow: hidden;
}

.medal-fact {
  display: flex;
  align-items: baseline;
  justify-content: space-between;
  gap: 16px;
  padding: 12px 16px;
}

.medal-fact + .medal-fact {
  border-top: 1px solid var(--line);
}

.medal-fact-value {
  margin: 0;
  font-family: var(--font-mono);
  font-size: 13px;
  color: var(--text-0);
  text-align: right;
}

/* ---- video ---- */

.medal-video {
  margin: 0;
  display: grid;
  gap: 10px;
}

.medal-video-el {
  width: 100%;
  aspect-ratio: 16 / 9;
  display: block;
  background: #000;
  border: 1px solid var(--line-strong);
  border-radius: var(--radius-lg);
  box-shadow: var(--shadow-medium);
}

.medal-caption {
  display: flex;
  justify-content: flex-end;
}

/* ---- gallery filmstrip ---- */

.medal-gallery {
  display: grid;
  grid-auto-flow: column;
  grid-auto-columns: minmax(220px, 1fr);
  gap: 12px;
  overflow-x: auto;
  scroll-snap-type: x proximity;
  padding-bottom: 6px;
  scrollbar-width: thin;
  scrollbar-color: var(--ink-3) transparent;
}

.medal-shot {
  margin: 0;
  display: grid;
  gap: 8px;
  scroll-snap-align: start;
}

.medal-shot-img {
  width: 100%;
  aspect-ratio: 4 / 3;
  object-fit: cover;
  border: 1px solid var(--line);
  border-radius: var(--radius-md);
  filter: grayscale(0.25) contrast(1.02);
  transition: filter var(--dur-fast), transform var(--dur-fast) var(--ease-elastic), border-color var(--dur-fast);
}

.medal-shot:hover .medal-shot-img {
  filter: grayscale(0);
  transform: translateY(-3px);
  border-color: var(--line-strong);
}

/* ---- collapsible coverage ---- */

.medal-coverage {
  border: 1px solid var(--line);
  border-radius: var(--radius-lg);
  background: var(--ink-1);
  overflow: hidden;
}

.medal-coverage-summary {
  display: flex;
  align-items: center;
  gap: 14px;
  padding: 16px 20px;
  cursor: pointer;
  list-style: none;
  transition: background var(--dur-fast);
}

.medal-coverage-summary::-webkit-details-marker {
  display: none;
}

.medal-coverage-summary:hover {
  background: var(--ink-2);
}

.medal-coverage-label {
  font-family: var(--font-display);
  font-size: 14px;
  font-weight: 700;
  color: var(--text-0);
}

.medal-coverage-hint {
  font-family: var(--font-mono);
  font-size: 11px;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  color: var(--text-3);
}

.medal-coverage-chevron {
  width: 18px;
  height: 18px;
  margin-left: auto;
  color: var(--text-2);
  transition: transform var(--dur-fast) var(--ease-out);
}

.medal-coverage[open] .medal-coverage-chevron {
  transform: rotate(180deg);
}

.medal-coverage-list {
  margin: 0;
  padding: 0;
  list-style: none;
  border-top: 1px solid var(--line);
}

.medal-coverage-item + .medal-coverage-item {
  border-top: 1px solid var(--line);
}

.medal-coverage-link {
  display: flex;
  align-items: center;
  gap: 16px;
  padding: 14px 20px;
  transition: background var(--dur-fast);
}

.medal-coverage-link:hover {
  background: var(--ink-2);
}

.medal-coverage-body {
  display: grid;
  gap: 3px;
  min-width: 0;
}

.medal-coverage-meta {
  font-family: var(--font-mono);
  font-size: 10.5px;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  color: var(--accent-warm);
}

.medal-coverage-title {
  font-family: var(--font-display);
  font-size: 14px;
  font-weight: 600;
  color: var(--text-0);
  line-height: 1.35;
}

.medal-coverage-note {
  font-size: 12.5px;
  color: var(--text-3);
}

.medal-coverage-ext {
  width: 16px;
  height: 16px;
  margin-left: auto;
  flex-shrink: 0;
  color: var(--text-3);
  transition: color var(--dur-fast), transform var(--dur-fast) var(--ease-out);
}

.medal-coverage-link:hover .medal-coverage-ext {
  color: var(--text-0);
  transform: translate(2px, -2px);
}

/* ---- cta ---- */

.medal-cta {
  justify-self: start;
}

@media (max-width: 960px) {
  .medal-head {
    grid-template-columns: 1fr;
  }
}

@media (max-width: 640px) {
  .medal-gallery {
    grid-auto-columns: minmax(170px, 78vw);
  }
}
</style>

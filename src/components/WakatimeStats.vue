<script setup lang="ts">
const stats = [
  {
    title: 'Languages & Frameworks',
    description: 'Top technologies used in recent development',
    src: 'https://wakatime.com/share/@018ed42e-558e-4b3d-8108-8afdc99705c2/185edccf-a90b-4067-88ed-c21a844876f8.svg',
  },
  {
    title: 'Operating systems',
    description: 'Top Operating systems used in recent development',
    src: 'https://wakatime.com/share/@018ed42e-558e-4b3d-8108-8afdc99705c2/9f4a0151-c9fc-4f12-b3ca-74af0409cd4b.svg',
  },
];


</script>

<template>
  <div class="wakatime reveal reveal--up">
    <div class="wakatime-intro">
<p class="wakatime-lede">
        Live metrics from <strong>WakaTime</strong> - no claims here, just what
        I've actually been building and how much time it takes.
      </p>
    </div>

    <div class="wakatime-grid">
      <figure v-for="(stat, i) in stats" :key="stat.title" class="wakatime-plate">
        <div class="wakatime-plate-head">
          <span class="datalabel">{{ stat.title }}</span>
          <span class="wakatime-sub">{{ stat.description }}</span>
        </div>
        <div class="wakatime-chart">
          <img
            :src="stat.src"
            :alt="stat.title"
            loading="lazy"
            style="width: 100%; height: auto; display: block;"
          />
        </div>
      </figure>
    </div>

    <p class="wakatime-note">
      <span class="datalabel">source</span>
      <span class="live-indicator">
        <span class="live-dot"></span>
        <span class="live-text">live</span>
      </span>
      <a :href="`https://wakatime.com/@${'018ed42e-558e-4b3d-8108-8afdc99705c2'}`" target="_blank" rel="noopener noreferrer">
        Full WakaTime profile
        <svg width="14" height="14" viewBox="0 0 14 14" fill="none" aria-hidden="true">
          <path d="M10.5 3.5L3.5 10.5M3.5 3.5H7.5V7.5" stroke="currentColor" stroke-width="1.25" stroke-linecap="round" stroke-linejoin="round"/>
        </svg>
      </a>
    </p>
  </div>
</template>

<style scoped>
.wakatime {
  margin-top: clamp(36px, 5vw, 52px);
  display: grid;
  gap: clamp(28px, 4vw, 44px);
}

.wakatime-intro {
  display: grid;
  gap: 12px;
}

.wakatime-lede {
  margin: 0;
  font-size: clamp(15px, 1.8vw, 18px);
  line-height: 1.65;
  color: var(--text-1);
  max-width: 680px;
}

.wakatime-lede strong {
  color: var(--text-0);
  font-weight: 600;
}

.wakatime-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 1px;
  background: var(--line);
  border: 1px solid var(--line);
  border-radius: var(--radius-lg);
  overflow: hidden;
}

.wakatime-plate {
  margin: 0;
  background: var(--ink-2);
  padding: 24px;
  display: grid;
  gap: 16px;
  transition: transform var(--dur-fast) var(--ease-elastic);
}

.wakatime-plate:hover {
  transform: translateY(-2px);
}

.wakatime-plate-head {
  display: grid;
  gap: 6px;
}

.wakatime-sub {
  font-size: 13px;
  color: var(--text-3);
  font-family: var(--font-body);
}

.wakatime-chart {
  background: var(--ink-1);
  border: 1px solid var(--line);
  border-radius: var(--radius-md);
  padding: 16px;
  min-height: 280px;
  display: grid;
  place-items: start center;
}

.wakatime-note {
  display: flex;
  align-items: center;
  gap: 12px;
  font-size: 13px;
  color: var(--text-3);
  margin-top: 8px;
  flex-wrap: wrap;
}

.wakatime-note a {
  color: var(--text-2);
  display: inline-flex;
  align-items: center;
  gap: 6px;
  transition: color var(--dur-fast) var(--ease-out);
}

.wakatime-note a:hover {
  color: var(--text-0);
}

.wakatime-note a svg {
  flex-shrink: 0;
}

.live-indicator {
  display: inline-flex;
  align-items: center;
  gap: 6px;
}

.live-dot {
  width: 6px;
  height: 6px;
  border-radius: 50%;
  background: var(--emerald);
  animation: livePulse 2s ease-in-out infinite;
}

@keyframes livePulse {
  0%, 100% { opacity: 1; transform: scale(1); }
  50% { opacity: 0.4; transform: scale(0.8); }
}

.live-text {
  font-family: var(--font-mono);
  font-size: 10px;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: var(--emerald);
}

@media (max-width: 768px) {
  .wakatime-grid {
    grid-template-columns: 1fr;
  }

  .wakatime-plate {
    padding: 20px;
  }

  .wakatime-chart {
    min-height: 240px;
    padding: 12px;
  }
}
</style>

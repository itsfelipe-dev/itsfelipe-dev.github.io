<script setup lang="ts">
import { onMounted, onBeforeUnmount, ref } from 'vue';

const props = withDefaults(defineProps<{ home?: boolean }>(), { home: true });

const icons: Record<string, string> = {
  user: '<path d="M20 21v-2a4 4 0 0 0-4-4H8a4 4 0 0 0-4 4v2"/><circle cx="12" cy="7" r="4"/>',
  activity: '<polyline points="22 12 18 12 15 21 9 3 6 12 2 12"/>',
  route: '<circle cx="6" cy="19" r="3"/><path d="M9 19h8.5a3.5 3.5 0 0 0 0-7h-11a3.5 3.5 0 0 1 0-7H15"/><circle cx="18" cy="5" r="3"/>',
  cpu: '<rect x="4" y="4" width="16" height="16" rx="2"/><rect x="9" y="9" width="6" height="6"/><path d="M15 2v2M15 20v2M2 15h2M2 9h2M20 15h2M20 9h2M9 2v2M9 20v2"/>',
  layers: '<path d="m12 2 10 6-10 6L2 8z"/><path d="m2 14 10 6 10-6"/><path d="m2 20 10 6 10-6"/>',
  briefcase: '<rect x="2" y="7" width="20" height="14" rx="2"/><path d="M16 21V5a2 2 0 0 0-2-2h-4a2 2 0 0 0-2 2v16"/>',
  award: '<circle cx="12" cy="8" r="6"/><path d="M15.477 12.89 17 22l-5-3-5 3 1.523-9.11"/>',
  target: '<circle cx="12" cy="12" r="10"/><circle cx="12" cy="12" r="6"/><circle cx="12" cy="12" r="2"/>',
  book: '<path d="M2 3h6a4 4 0 0 1 4 4v14a3 3 0 0 0-3-3H2z"/><path d="M22 3h-6a4 4 0 0 0-4 4v14a3 3 0 0 1 3-3h7z"/>',
  chart: '<line x1="12" y1="20" x2="12" y2="10"/><line x1="18" y1="20" x2="18" y2="4"/><line x1="6" y1="20" x2="6" y2="16"/>',
  mail: '<rect x="2" y="4" width="20" height="16" rx="2"/><path d="m22 7-8.97 5.7a1.94 1.94 0 0 1-2.06 0L2 7"/>',
};

const sections = [
  { id: 'top', label: 'Overview', icon: 'user' },
  { id: 'lineage', label: 'Pipeline', icon: 'activity' },
  { id: 'experience', label: 'Experience', icon: 'route' },
  { id: 'architecture', label: 'Architecture', icon: 'cpu' },
  { id: 'stack', label: 'Tools', icon: 'layers' },
  { id: 'skills', label: 'Skills', icon: 'target' },
  { id: 'education', label: 'Education', icon: 'book' },
  { id: 'case-studies', label: 'Projects', icon: 'briefcase' },
  { id: 'writing', label: 'Notes', icon: 'book' },
  { id: 'worldskills', label: 'WorldSkills', icon: 'award' },
  { id: 'awards', label: 'Awards', icon: 'award' },
  { id: 'wakatime', label: 'Activity', icon: 'chart' },
  { id: 'contact', label: 'Contact', icon: 'mail' },
];

const active = ref('top');
const isDark = ref(false);
const progress = ref(0);
const navOpen = ref(false);
const toggleBounce = ref(false);
let observer: IntersectionObserver | null = null;

function onScroll() {
  const st = window.scrollY;
  const max = document.documentElement.scrollHeight - window.innerHeight;
  progress.value = max > 0 ? st / max : 0;

  const probes = sections
    .map((s) => document.getElementById(s.id))
    .filter((el): el is HTMLElement => el !== null);
  let current = sections[0].id;
  for (const el of probes) {
    if (el.getBoundingClientRect().top <= 120) current = el.id;
  }
  active.value = current;
}

function toggleTheme() {
  isDark.value = !isDark.value;
  document.documentElement.setAttribute('data-theme', isDark.value ? 'dark' : 'light');
  localStorage.setItem('theme', isDark.value ? 'dark' : 'light');
}

function handleToggle() {
  toggleBounce.value = true;
  setTimeout(() => { toggleBounce.value = false; }, 300);
  toggleTheme();
}

function onKeydown(e: KeyboardEvent) {
  if (e.key === 'Escape' && navOpen.value) navOpen.value = false;
}

function closeNav() {
  navOpen.value = false;
}

onMounted(() => {
  const savedTheme = localStorage.getItem('theme');
  const prefersDark = window.matchMedia('(prefers-color-scheme: dark)').matches;
  const initialTheme = savedTheme || (prefersDark ? 'dark' : 'light');

  if (initialTheme === 'dark') {
    isDark.value = true;
    document.documentElement.setAttribute('data-theme', 'dark');
  }

  onScroll();
  window.addEventListener('scroll', onScroll, { passive: true });
  window.addEventListener('resize', onScroll, { passive: true });
  document.addEventListener('keydown', onKeydown);
  observer = new IntersectionObserver(
    (entries) => {
      for (const e of entries) {
        if (e.isIntersecting) active.value = e.target.id;
      }
    },
    { rootMargin: '-40% 0px -55% 0px' },
  );
  for (const s of sections) {
    const el = document.getElementById(s.id);
    if (el) observer.observe(el);
  }
});

onBeforeUnmount(() => {
  window.removeEventListener('scroll', onScroll);
  window.removeEventListener('resize', onScroll);
  document.removeEventListener('keydown', onKeydown);
  observer?.disconnect();
});
</script>

<template>
  <header class="header">
    <div class="wrap header-inner">
      <a :href="props.home ? '#top' : '/#top'" class="header-brand" aria-label="Andres Felipe - data engineer">
        <span class="header-word">Andres Felipe</span>
        <span class="header-dot" aria-hidden="true"></span>
        <span class="header-code code-strip" aria-hidden="true">
          <!-- <span class="code-block code-block--emerald code-block--dim"></span>
          <span class="code-block code-block--cyan"></span>
          <span class="code-block code-block--violet code-block--dim"></span> -->
        </span>
      </a>

      <nav class="header-nav" aria-label="Page index">
        <a
          v-for="s in sections"
          :key="s.id"
          :href="`${props.home ? '' : '/'}#${s.id}`"
          class="nav-entry"
          :class="{ 'nav-entry--active': active === s.id }"
          :aria-current="active === s.id ? 'true' : undefined"
          :aria-label="s.label"
          :title="s.label"
          @click="closeNav"
        >
          <svg
            class="nav-icon"
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            stroke-width="1.8"
            stroke-linecap="round"
            stroke-linejoin="round"
            aria-hidden="true"
            v-html="icons[s.icon]"
          ></svg>
          <span class="nav-label">{{ s.label }}</span>
        </a>
      </nav>

      <div class="header-actions">
        <button class="theme-toggle" @click="handleToggle" :aria-label="isDark ? 'Switch to light theme' : 'Switch to dark theme'" :class="{ 'toggling': toggleBounce }">
          <svg v-if="!isDark" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true">
            <circle cx="12" cy="12" r="5" />
            <line x1="12" y1="1" x2="12" y2="3" />
            <line x1="12" y1="21" x2="12" y2="23" />
            <line x1="4.22" y1="4.22" x2="5.64" y2="5.64" />
            <line x1="18.36" y1="18.36" x2="19.78" y2="19.78" />
            <line x1="1" y1="12" x2="3" y2="12" />
            <line x1="21" y1="12" x2="23" y2="12" />
            <line x1="4.22" y1="19.78" x2="5.64" y2="18.36" />
            <line x1="18.36" y1="5.64" x2="19.78" y2="4.22" />
          </svg>
          <svg v-else viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true">
            <path d="M21 12.79A9 9 0 1 1 11.21 3 7 7 0 0 0 21 12.79z" />
          </svg>
        </button>
        <a href="/andres-orjuela-resume.pdf" class="btn btn-primary header-resume" download>
          <svg width="12" height="12" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true">
            <path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4" />
            <polyline points="7 10 12 15 17 10" />
            <line x1="12" y1="15" x2="12" y2="3" />
          </svg>
          <span>Resume</span>
        </a>
        <button class="menu-toggle" @click="navOpen = !navOpen" :aria-expanded="navOpen" :aria-label="navOpen ? 'Close menu' : 'Open menu'">
          <svg v-if="!navOpen" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" aria-hidden="true">
            <line x1="4" y1="7" x2="20" y2="7" />
            <line x1="4" y1="12" x2="20" y2="12" />
            <line x1="4" y1="17" x2="20" y2="17" />
          </svg>
          <svg v-else viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" aria-hidden="true">
            <line x1="6" y1="6" x2="18" y2="18" />
            <line x1="18" y1="6" x2="6" y2="18" />
          </svg>
        </button>
      </div>
    </div>

    <div class="header-progress" aria-hidden="true">
      <span class="header-progress-bar" :style="{ transform: `scaleX(${progress})` }"></span>
    </div>

    <Transition name="menu">
      <nav v-if="navOpen" class="header-mobile" aria-label="Page index">
        <a
          v-for="s in sections"
          :key="s.id"
          :href="`${props.home ? '' : '/'}#${s.id}`"
          class="mobile-entry"
          :class="{ 'mobile-entry--active': active === s.id }"
          :aria-current="active === s.id ? 'true' : undefined"
          @click="closeNav"
        >
          <svg
            class="nav-icon"
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            stroke-width="1.8"
            stroke-linecap="round"
            stroke-linejoin="round"
            aria-hidden="true"
            v-html="icons[s.icon]"
          ></svg>
          <span class="mobile-label">{{ s.label }}</span>
        </a>
      </nav>
    </Transition>
  </header>
</template>

<style scoped>
.header {
  position: sticky;
  top: 0;
  z-index: 100;
  background: var(--ink-0);
  border-bottom: 1px solid var(--line);
  animation: headerIn 0.7s var(--ease-out) both;
}

@keyframes headerIn {
  from {
    opacity: 0;
    transform: translateY(-10px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.header-inner {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 24px;
  height: 72px;
  max-width: none;
}

.header-brand {
  display: flex;
  align-items: center;
  gap: 10px;
  flex-shrink: 0;
}

.header-word {
  font-family: var(--font-display);
  font-size: 15px;
  font-weight: 800;
  letter-spacing: 0.01em;
  white-space: nowrap;
  color: var(--text-0);
}

.header-dot {
  width: 5px;
  height: 5px;
  border-radius: 50%;
  background: var(--accent-warm);
  flex-shrink: 0;
}

.header-code {
  display: none;
}

.header-nav {
  display: flex;
  align-items: center;
  gap: 2px;
}

.nav-entry {
  display: inline-flex;
  align-items: center;
  gap: 7px;
  padding: 7px 10px;
  border-radius: var(--radius-full);
  transition: all var(--dur-fast) var(--ease-elastic);
}

.nav-entry:hover {
  background: var(--ink-2);
  transform: translateY(-1px);
}

.nav-icon {
  width: 15px;
  height: 15px;
  flex-shrink: 0;
}

.nav-label {
  font-family: var(--font-display);
  font-size: 11.5px;
  font-weight: 700;
  letter-spacing: 0.02em;
  color: var(--text-2);
  transition: color var(--dur-fast);
}

.nav-entry--active {
  background: var(--accent-warm);
}

.nav-entry--active .nav-icon,
.nav-entry--active .nav-label {
  color: var(--ink-0);
}

.header-actions {
  display: flex;
  align-items: center;
  gap: 12px;
  flex-shrink: 0;
}

.header-resume {
  padding: 10px 16px;
  font-size: 12px;
  font-weight: 700;
  border-radius: var(--radius-full);
}

.theme-toggle,
.menu-toggle {
  width: 40px;
  height: 40px;
  border-radius: 50%;
  border: 1px solid var(--line-strong);
  background: var(--ink-1);
  display: grid;
  place-items: center;
  color: var(--text-1);
  transition: transform var(--dur-fast) var(--ease-elastic), background var(--dur-fast), border-color var(--dur-fast);
}

.theme-toggle:hover,
.menu-toggle:hover {
  transform: scale(1.08);
  background: var(--ink-2);
  border-color: var(--accent-warm);
}

.theme-toggle svg,
.menu-toggle svg {
  width: 19px;
  height: 19px;
}

.theme-toggle.toggling {
  animation: themeBounce 0.4s var(--ease-elastic);
}

@keyframes themeBounce {
  0% { transform: scale(1) rotate(0deg); }
  25% { transform: scale(1.2) rotate(-15deg); }
  50% { transform: scale(0.9) rotate(10deg); }
  75% { transform: scale(1.1) rotate(-5deg); }
  100% { transform: scale(1) rotate(0deg); }
}

.menu-toggle {
  display: none;
}

/* ---- scroll progress ---- */

.header-progress {
  position: absolute;
  left: 0;
  right: 0;
  bottom: -1px;
  height: 2px;
  pointer-events: none;
}

.header-progress-bar {
  display: block;
  width: 100%;
  height: 100%;
  background: linear-gradient(90deg, var(--accent-warm), var(--emerald));
  transform-origin: left center;
  transform: scaleX(0);
  transition: transform 60ms linear;
}

/* ---- mobile panel ---- */

.header-mobile {
  position: absolute;
  top: 100%;
  left: 0;
  right: 0;
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 2px;
  border: 1px solid var(--line);
  border-top: none;
  border-radius: 0 0 var(--radius-lg) var(--radius-lg);
  margin: 0 auto;
  max-width: var(--max-width);
  padding: 2px;
  background-color: var(--ink-1);
  box-shadow: var(--shadow-medium);
}

.mobile-entry {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 14px 12px;
  border-radius: var(--radius-md);
  background: var(--ink-1);
  transition: background var(--dur-fast), transform var(--dur-fast) var(--ease-elastic);
}

.mobile-entry:hover {
  background: var(--ink-2);
}

.mobile-entry--active {
  background: var(--accent-warm);
}

.mobile-entry--active .nav-icon,
.mobile-entry--active .mobile-label {
  color: var(--ink-0);
}

.mobile-entry .nav-icon {
  width: 18px;
  height: 18px;
  color: var(--accent-warm);
}

.mobile-entry--active .nav-icon {
  color: var(--ink-0);
}

.mobile-label {
  font-family: var(--font-display);
  font-size: 13px;
  font-weight: 700;
  color: var(--text-1);
}

.menu-enter-active,
.menu-leave-active {
  transition: opacity 0.22s var(--ease-out), transform 0.22s var(--ease-out);
}

.menu-enter-from,
.menu-leave-to {
  opacity: 0;
  transform: translateY(-8px);
}

@media (min-width: 880px) {
  .header-code {
    display: inline-flex;
  }
}

@media (max-width: 1399px) {
  .nav-label {
    display: none;
  }
}

@media (max-width: 760px) {
  .header-nav {
    display: none;
  }

  .menu-toggle {
    display: grid;
  }

  .header-mobile {
    grid-template-columns: 1fr;
  }
}

@media (max-width: 420px) {
  .header-word {
    font-size: 13px;
  }

  .header-resume span {
    display: none;
  }

  .header-resume {
    padding: 10px 12px;
  }
}
</style>

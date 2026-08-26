<script setup lang="ts">
const props = withDefaults(defineProps<{ base?: string }>(), { base: '' });

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

const index = [
  { label: 'Overview', id: 'top', icon: 'user' },
  { label: 'Pipeline', id: 'lineage', icon: 'activity' },
  { label: 'Experience', id: 'experience', icon: 'route' },
  { label: 'Architecture', id: 'architecture', icon: 'cpu' },
  { label: 'Tools', id: 'stack', icon: 'layers' },
  { label: 'Skills', id: 'skills', icon: 'target' },
  { label: 'Education', id: 'education', icon: 'book' },
  { label: 'Projects', id: 'case-studies', icon: 'briefcase' },
  { label: 'Field Notes', id: 'writing', icon: 'book' },
  { label: 'WorldSkills', id: 'worldskills', icon: 'award' },
  { label: 'Awards', id: 'awards', icon: 'award' },
  { label: 'Coding activity', id: 'wakatime', icon: 'chart' },
  { label: 'Contact', id: 'contact', icon: 'mail' },
];

const year = new Date().getFullYear();
</script>

<template>
  <footer class="footer">
    <div class="wrap footer-inner reveal reveal--up">
      <div class="footer-top" style="--d: 0">
        <div class="footer-brand">
          <span class="footer-word">Andres Felipe</span>
          <span class="footer-colophon">data engineering - end of record</span>
        </div>

        <nav class="footer-links" aria-label="Page index">
          <a v-for="i in index" :key="i.id" :href="`${props.base}#${i.id}`" class="footer-link">
            <svg
              class="footer-icon"
              viewBox="0 0 24 24"
              fill="none"
              stroke="currentColor"
              stroke-width="1.8"
              stroke-linecap="round"
              stroke-linejoin="round"
              aria-hidden="true"
              v-html="icons[i.icon]"
            ></svg>
            <span class="footer-label">{{ i.label }}</span>
          </a>
        </nav>
      </div>

      <div class="footer-bottom" style="--d: 1">
        <span class="footer-copy">&copy; {{ year }} Andres Felipe Orjuela Arias</span>
      </div>
    </div>
  </footer>
</template>

<style scoped>
.footer {
  border-top: 1px solid var(--line);
  padding: clamp(40px, 6vw, 64px) 0 clamp(28px, 4vw, 40px);
}

.footer-inner {
  display: flex;
  flex-direction: column;
  gap: 30px;
}

.footer-top {
  display: flex;
  align-items: flex-start;
  justify-content: space-between;
  gap: 24px;
  flex-wrap: wrap;
}

.footer-brand {
  display: flex;
  align-items: baseline;
  gap: 14px;
}

.footer-word {
  font-family: var(--font-display);
  font-size: 18px;
  font-weight: 700;
  letter-spacing: 0.01em;
  color: var(--text-0);
}

.footer-colophon {
  font-family: var(--font-mono);
  font-size: 11px;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: var(--text-3);
}

.footer-links {
  display: flex;
  gap: 2px;
  flex-wrap: wrap;
}

.footer-link {
  display: inline-flex;
  align-items: center;
  gap: 7px;
  padding: 8px 10px;
  border-radius: var(--radius-md);
  transition: all var(--dur-fast) var(--ease-elastic);
}

.footer-link:hover {
  background: var(--ink-2);
  transform: translateY(-1px);
}

.footer-icon {
  width: 14px;
  height: 14px;
  flex-shrink: 0;
  color: var(--text-3);
  transition: color var(--dur-fast);
}

.footer-link:hover .footer-icon {
  color: var(--accent-warm);
}

.footer-label {
  font-family: var(--font-display);
  font-size: 12px;
  font-weight: 600;
  letter-spacing: 0.02em;
  color: var(--text-2);
}

.footer-bottom {
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap;
  gap: 12px;
  padding-top: 24px;
  border-top: 1px solid var(--line);
}

.footer-copy {
  font-size: 13px;
  color: var(--text-3);
}

.footer-tech {
  font-family: var(--font-mono);
  font-size: 11.5px;
  letter-spacing: 0.04em;
  color: var(--text-3);
}

@media (max-width: 640px) {
  .footer-top {
    flex-direction: column;
  }

  .footer-bottom {
    flex-direction: column;
    align-items: flex-start;
  }
}
</style>

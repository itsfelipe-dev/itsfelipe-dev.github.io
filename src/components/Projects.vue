<script setup lang="ts">
interface CaseStudy {
  title: string;
  timeframe: string;
  summary: string;
  architecture: string[];
  outcome: string;
  stack: string[];
  link?: string;
  linkLabel?: string;
  accent: 'emerald' | 'cyan' | 'violet';
  featured?: boolean;
}

interface Props {
  items: CaseStudy[];
}

defineProps<Props>();

function slug(title: string): string {
  return title.toLowerCase().replace(/[^a-z0-9]+/g, '-').replace(/^-|-$/g, '');
}
</script>

<template>
  <div class="works reveal reveal--up">
    <article
      v-for="(p, i) in items"
      :key="p.title"
      class="work"
      :class="[`work--${p.accent}`, { 'work--featured': p.featured }]"
      :style="{ '--d': i }"
    >
      <div class="work-head">
        <div class="work-no">
          <span v-if="p.featured" class="work-feature">
            <span class="code-block code-block--violet"></span>
            <span class="datalabel">featured</span>
          </span>
        </div>
        <div class="work-meta">
          <span class="work-timeframe">{{ p.timeframe }}</span>
          <span class="work-path">{{ slug(p.title) }}</span>
        </div>
      </div>

      <h3 class="work-title">{{ p.title }}</h3>
      <p class="work-summary">{{ p.summary }}</p>

      <div class="work-arch">
        <p class="datalabel">architecture</p>
        <ol class="work-steps">
          <li v-for="(step, k) in p.architecture" :key="k" class="work-step">
            <span class="work-step-mark" aria-hidden="true"></span>
            <span class="work-step-text">{{ step }}</span>
          </li>
        </ol>
      </div>

      <div class="work-outcome">
        <p class="datalabel" :class="`datalabel--${p.accent}`">outcome</p>
        <p class="work-outcome-text">{{ p.outcome }}</p>
      </div>

      <div class="work-foot">
        <div class="work-stack">
          <span v-for="s in p.stack" :key="s" class="work-chip">{{ s }}</span>
        </div>
        <a v-if="p.link" :href="p.link" target="_blank" rel="noopener" class="work-link">
          <span>{{ p.linkLabel ?? 'View on GitHub' }}</span>
          <svg width="12" height="12" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true">
            <line x1="7" y1="17" x2="17" y2="7" />
            <polyline points="7 7 17 7 17 17" />
          </svg>
        </a>
      </div>
    </article>
  </div>
</template>

<style scoped>
.works {
  margin-top: clamp(36px, 5vw, 52px);
  display: grid;
  gap: 1px;
  background: var(--line);
  border: 1px solid var(--line);
  border-radius: var(--radius-plate);
  overflow: hidden;
}

.work {
  background: var(--ink-1);
  padding: clamp(24px, 4vw, 36px);
  display: grid;
  gap: 16px;
  position: relative;
  overflow: hidden;
  transition: background var(--dur-fast);
}

.work::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 2px;
  background: var(--accent, var(--line));
  opacity: 0.5;
  transition: height var(--dur-fast), opacity var(--dur-fast);
}

.work:hover {
  background: var(--ink-2);
}

.work:hover::before {
  height: 3px;
  opacity: 1;
}

.work--featured {
  background: var(--ink-2);
}

.work--featured::before {
  height: 3px;
  opacity: 1;
}

.work-head {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  gap: 16px;
}

.work-no {
  display: flex;
  align-items: center;
  gap: 14px;
}

.work-feature {
  display: inline-flex;
  align-items: center;
  gap: 7px;
}

.work-meta {
  display: flex;
  align-items: center;
  gap: 14px;
  flex-wrap: wrap;
}

.work-timeframe {
  font-family: var(--font-mono);
  font-size: 11px;
  letter-spacing: 0.06em;
  color: var(--text-3);
}

.work-path {
  font-family: var(--font-mono);
  font-size: 10.5px;
  color: var(--text-3);
}

.work-title {
  font-family: var(--font-display);
  font-size: clamp(20px, 3vw, 28px);
  font-weight: 600;
  letter-spacing: 0;
  line-height: 1.2;
  color: var(--text-0);
  margin: 0;
  transition: color var(--dur-fast);
}

.work:hover .work-title {
  color: var(--accent, var(--text-0));
}

.work-summary {
  margin: 0;
  font-size: 15px;
  line-height: 1.65;
  color: var(--text-1);
  max-width: 72ch;
}

.work-arch {
  display: grid;
  gap: 12px;
  padding-top: 6px;
}

.work-steps {
  margin: 0;
  padding: 0;
  list-style: none;
  display: grid;
  gap: 8px;
}

.work-step {
  display: flex;
  align-items: flex-start;
  gap: 12px;
  font-size: 13.5px;
  color: var(--text-1);
  line-height: 1.55;
}

.work-step-mark {
  width: 8px;
  height: 8px;
  border-radius: var(--radius-full);
  background: var(--line-accent);
  flex-shrink: 0;
  margin-top: 7px;
}

.work-step-text {
  flex: 1;
}

.work-outcome {
  border-left: 2px solid var(--line-accent);
  padding-left: 18px;
  display: grid;
  gap: 6px;
}

.datalabel--emerald { color: var(--emerald); }
.datalabel--cyan { color: var(--cyan); }
.datalabel--violet { color: var(--violet); }

.work-outcome-text {
  margin: 0;
  font-size: 14px;
  color: var(--text-0);
  line-height: 1.6;
  font-weight: 500;
  max-width: 72ch;
}

.work-foot {
  margin-top: 6px;
  padding-top: 18px;
  border-top: 1px solid var(--line);
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 12px;
  flex-wrap: wrap;
}

.work-stack {
  display: flex;
  flex-wrap: wrap;
  gap: 6px;
}

.work-chip {
  font-family: var(--font-mono);
  font-size: 11px;
  letter-spacing: 0.02em;
  color: var(--text-2);
  border: 1px solid var(--line);
  padding: 4px 10px;
  border-radius: var(--radius-plate);
}

.work-link {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  font-family: var(--font-mono);
  font-size: 12px;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: var(--text-0);
  transition: color var(--dur-fast);
  white-space: nowrap;
}

.work-link svg {
  transition: transform var(--dur-fast) var(--ease-out);
}

.work-link:hover {
  color: var(--accent, var(--cyan));
}

.work-link:hover svg {
  transform: translate(2px, -2px);
}

.work--emerald { --accent: var(--emerald); }
.work--cyan { --accent: var(--cyan); }
.work--violet { --accent: var(--violet); }

@media (max-width: 640px) {
  .work-head {
    flex-direction: column;
  }
}
</style>

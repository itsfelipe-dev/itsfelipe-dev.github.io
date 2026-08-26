<script setup lang="ts">
interface Category {
  label: string;
  items: string[];
  accent: 'emerald' | 'cyan' | 'violet';
  icon?: string;
}

interface Props {
  categories: Category[];
}

defineProps<Props>();

function slug(label: string): string {
  return label.toLowerCase().replace(/[^a-z0-9]+/g, '-');
}
</script>

<template>
  <div class="manifests reveal reveal--up">
    <div
      v-for="(c, i) in categories"
      :key="c.label"
      class="manifest"
      :class="`manifest--${c.accent}`"
      :style="{ '--d': i }"
    >
      <div class="manifest-head">
        <span class="code-strip" aria-hidden="true">
          <span class="code-block code-block--dim" :class="`code-block--${c.accent}`"></span>
        </span>
        <h3 class="manifest-title">{{ c.label }}</h3>
        <span class="manifest-path">{{ slug(c.label) }}.txt</span>
      </div>
      <div class="manifest-body">
        <div v-for="item in c.items" :key="item" class="manifest-row">
          <span class="manifest-mark" aria-hidden="true">/</span>
          <span class="manifest-item">{{ item }}</span>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.manifests {
  margin-top: clamp(36px, 5vw, 52px);
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 1px;
  background: var(--line);
  border: 1px solid var(--line);
  border-radius: var(--radius-lg);
  overflow: hidden;
}

.manifest {
  background: var(--ink-1);
  display: flex;
  flex-direction: column;
  padding: 20px 22px;
  gap: 14px;
  position: relative;
  overflow: hidden;
  transition: background var(--dur-fast);
}

.manifest::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 2px;
  background: var(--accent, var(--line));
  opacity: 0.6;
  transition: height 0.2s var(--dur-fast), opacity 0.2s var(--dur-fast);
}

.manifest:hover {
  background: var(--ink-2);
}

.manifest:hover::before {
  height: 3px;
  opacity: 1;
}

.manifest:hover .manifest-title {
  color: var(--accent, var(--text-0));
}

.manifest--emerald { --accent: var(--emerald); }
.manifest--cyan { --accent: var(--cyan); }
.manifest--violet { --accent: var(--violet); }

.manifest:hover .manifest-mark {
  color: var(--accent, var(--text-2));
  transition: color var(--dur-fast);
}

.manifest:hover .manifest-item {
  color: var(--text-0);
  transition: color var(--dur-fast);
}

.manifest-head {
  display: grid;
  gap: 8px;
}

.manifest-title {
  font-family: var(--font-display);
  font-size: 15px;
  font-weight: 600;
  letter-spacing: 0;
  color: var(--text-0);
  margin: 0;
  line-height: 1.3;
  transition: color var(--dur-fast);
}

.manifest-path {
  font-family: var(--font-mono);
  font-size: 10.5px;
  letter-spacing: 0.08em;
  color: var(--text-3);
}

.manifest-body {
  display: grid;
  gap: 6px;
}

.manifest-row {
  display: flex;
  align-items: baseline;
  gap: 10px;
}

.manifest-mark {
  font-family: var(--font-mono);
  font-size: 12px;
  color: var(--text-3);
  flex-shrink: 0;
  transition: color var(--dur-fast);
}

.manifest-item {
  font-family: var(--font-mono);
  font-size: 12.5px;
  color: var(--text-1);
  line-height: 1.5;
  transition: color var(--dur-fast);
}

@media (max-width: 640px) {
  .manifests {
    grid-template-columns: 1fr;
  }
}
</style>

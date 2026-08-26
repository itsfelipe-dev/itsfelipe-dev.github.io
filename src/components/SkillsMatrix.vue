<script setup lang="ts">
interface Skill {
  name: string;
  level: 1 | 2 | 3 | 4;
}

interface Group {
  label: string;
  accent: 'emerald' | 'cyan' | 'violet';
  skills: Skill[];
}

interface Props {
  groups: Group[];
}

defineProps<Props>();

const LEVEL_LABEL: Record<number, string> = {
  1: 'hands-on',
  2: 'frequent',
  3: 'production',
  4: 'daily driver',
};

const LEVELS = [1, 2, 3, 4] as const;

function slug(label: string): string {
  return label.toLowerCase().replace(/[^a-z0-9]+/g, '-');
}
</script>

<template>
  <div class="matrix reveal reveal--up">
    <section
      v-for="(g, i) in groups"
      :key="g.label"
      class="matrix-group"
      :class="`matrix-group--${g.accent}`"
      :style="{ '--d': i }"
    >
      <div class="matrix-head">
        <span class="code-strip" aria-hidden="true">
          <span class="code-block code-block--dim" :class="`code-block--${g.accent}`"></span>
        </span>
        <h3 class="matrix-title">{{ g.label }}</h3>
        <span class="matrix-path">{{ slug(g.label) }}.prog</span>
      </div>

      <div class="matrix-rows">
        <div v-for="s in g.skills" :key="s.name" class="matrix-row">
          <span class="matrix-skill">{{ s.name }}</span>
          <div class="matrix-right">
            <span
              class="matrix-meter"
              role="img"
              :aria-label="`${s.name} - ${LEVEL_LABEL[s.level]}`"
            >
              <span
                v-for="lvl in LEVELS"
                :key="lvl"
                class="matrix-cell"
                :class="{ 'matrix-cell--on': lvl <= s.level }"
                :style="{ '--i': lvl }"
              ></span>
            </span>
            <span class="matrix-level">{{ LEVEL_LABEL[s.level] }}</span>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<style scoped>
.matrix {
  margin-top: clamp(36px, 5vw, 52px);
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 1px;
  background: var(--line);
  border: 1px solid var(--line);
  border-radius: var(--radius-lg);
  overflow: hidden;
}

.matrix-group {
  background: var(--ink-1);
  display: flex;
  flex-direction: column;
  padding: 20px 22px;
  gap: 16px;
  position: relative;
  overflow: hidden;
  transition: background var(--dur-fast);
}

.matrix-group::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 2px;
  background: var(--accent, var(--line));
  opacity: 0.6;
}

.matrix-group:hover {
  background: var(--ink-2);
}

.matrix-group--emerald { --accent: var(--emerald); }
.matrix-group--cyan { --accent: var(--cyan); }
.matrix-group--violet { --accent: var(--violet); }

.matrix-head {
  display: grid;
  gap: 8px;
}

.matrix-title {
  font-family: var(--font-display);
  font-size: 15px;
  font-weight: 600;
  color: var(--text-0);
  margin: 0;
  line-height: 1.3;
  transition: color var(--dur-fast);
}

.matrix-group:hover .matrix-title {
  color: var(--accent, var(--text-0));
}

.matrix-path {
  font-family: var(--font-mono);
  font-size: 10.5px;
  letter-spacing: 0.08em;
  color: var(--text-3);
}

.matrix-rows {
  display: grid;
  gap: 13px;
}

.matrix-row {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 14px;
}

.matrix-skill {
  font-family: var(--font-mono);
  font-size: 12px;
  color: var(--text-1);
  line-height: 1.4;
  min-width: 0;
  transition: color var(--dur-fast);
}

.matrix-group:hover .matrix-skill {
  color: var(--text-0);
}

.matrix-right {
  display: flex;
  align-items: center;
  gap: 10px;
  flex-shrink: 0;
}

.matrix-meter {
  display: inline-flex;
  gap: 3px;
}

.matrix-cell {
  width: 12px;
  height: 8px;
  border-radius: 2px;
  background: var(--ink-3);
  transition: background var(--dur-fast);
}

.matrix-cell--on {
  background: var(--accent, var(--line-accent));
}

.reveal.revealed .matrix-cell--on {
  animation: cellIn 0.4s var(--ease-out) both;
  animation-delay: calc(var(--d, 0) * 70ms + 0.45s + var(--i, 1) * 55ms);
}

@keyframes cellIn {
  from {
    opacity: 0;
    transform: scale(0.45);
  }
  to {
    opacity: 1;
    transform: scale(1);
  }
}

.matrix-level {
  font-family: var(--font-mono);
  font-size: 9.5px;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  color: var(--text-3);
  width: 72px;
  text-align: right;
}

@media (max-width: 640px) {
  .matrix {
    grid-template-columns: 1fr;
  }
}
</style>

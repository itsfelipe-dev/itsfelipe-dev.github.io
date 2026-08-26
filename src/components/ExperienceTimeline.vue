<script setup lang="ts">
interface Role {
  company: string;
  position: string;
  location: string;
  period: string;
  impact: string[];
  current?: boolean;
  companyInitials?: string;
}

interface Props {
  roles: Role[];
}

defineProps<Props>();
</script>

<template>
  <ol class="record reveal reveal--up">
    <li
      v-for="(r, i) in roles"
      :key="r.company + r.period"
      class="entry"
      :class="{ 'entry--current': r.current }"
      :style="{ '--d': i }"
    >
      <div class="entry-main">
        <div class="entry-head">
          <div class="entry-identity">
            <h3 class="entry-position">{{ r.position }}</h3>
            <p class="entry-company">{{ r.company }}<span class="entry-dot" aria-hidden="true">·</span>{{ r.location }}</p>
          </div>
          <div class="entry-meta">
            <span class="entry-period">{{ r.period }}</span>
            <span v-if="r.current" class="entry-active">
              <span class="code-block code-block--emerald"></span>
              <span class="datalabel">current</span>
            </span>
          </div>
        </div>

        <ul class="entry-impact">
          <li v-for="(line, k) in r.impact" :key="k" class="entry-impact-line" v-html="line"></li>
        </ul>
      </div>
    </li>
  </ol>
</template>

<style scoped>
.record {
  list-style: none;
  margin: clamp(36px, 5vw, 52px) 0 0;
  padding: 0;
  display: grid;
  gap: 16px;
}

.entry {
  display: grid;
  gap: 0;
  padding: 24px 22px;
  background: var(--ink-1);
  border-radius: var(--radius-lg);
  border: 1px solid var(--line);
  transition: all var(--dur-fast) var(--ease-elastic);
  box-shadow: var(--shadow-soft);
}

.entry:hover {
  transform: translateY(-2px);
  box-shadow: var(--shadow-medium);
  border-color: var(--line-strong);
}

.entry--current {
  border-color: var(--emerald);
  background: rgba(107, 142, 107, 0.05);
}

.entry--current:hover {
  background: rgba(107, 142, 107, 0.1);
}

.entry-main {
  display: grid;
  gap: 14px;
}

.entry-head {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  gap: 16px;
  flex-wrap: wrap;
}

.entry-identity {
  display: grid;
  gap: 4px;
}

.entry-position {
  font-family: var(--font-display);
  font-size: clamp(17px, 2.2vw, 22px);
  font-weight: 700;
  letter-spacing: -0.01em;
  line-height: 1.3;
  color: var(--text-0);
  margin: 0;
  transition: color var(--dur-fast);
}

.entry:hover .entry-position {
  color: var(--accent-warm);
}

.entry-company {
  margin: 0;
  font-family: var(--font-mono);
  font-size: 12.5px;
  letter-spacing: 0.02em;
  color: var(--text-2);
}

.entry-dot {
  margin: 0 9px;
  color: var(--text-3);
}

.entry-meta {
  display: flex;
  align-items: center;
  gap: 14px;
  flex-shrink: 0;
  padding-top: 4px;
}

.entry-period {
  font-family: var(--font-mono);
  font-size: 11.5px;
  letter-spacing: 0.04em;
  color: var(--text-3);
  white-space: nowrap;
}

.entry-active {
  display: inline-flex;
  align-items: center;
  gap: 7px;
  padding: 4px 10px;
  border-radius: var(--radius-full);
  background: rgba(107, 142, 107, 0.1);
  border: 1px solid var(--emerald);
}

.entry-active .code-block {
  background: var(--emerald);
}

.entry-impact {
  margin: 0;
  padding: 0;
  list-style: none;
  display: grid;
  gap: 8px;
}

.entry-impact-line {
  position: relative;
  padding-left: 18px;
  font-size: 14px;
  line-height: 1.6;
  color: var(--text-1);
}

.entry-impact-line::before {
  content: '';
  position: absolute;
  left: 2px;
  top: 10px;
  width: 8px;
  height: 1px;
  background: var(--line-accent);
}

.entry-impact :deep(strong) {
  color: var(--text-0);
  font-weight: 600;
}

@media (max-width: 640px) {
  .entry {
    padding: 16px;
  }

  .entry-meta {
    width: 100%;
    justify-content: space-between;
  }
}
</style>
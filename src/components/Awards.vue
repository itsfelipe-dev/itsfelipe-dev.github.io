<script setup lang="ts">
interface Award {
  rank: string;
  title: string;
  detail: string;
  place: string;
  period: string;
  accent?: 'emerald' | 'cyan' | 'violet';
}

defineProps<{ awards: Award[] }>();
</script>

<template>
  <div class="awards reveal reveal--up">
    <ol class="awards-list">
      <li
        v-for="(a, i) in awards"
        :key="a.title"
        class="award"
        :style="{ '--d': i }"
      >
        <div class="award-head">
          <span class="award-rank">{{ a.rank }}</span>
          <span class="award-period">{{ a.period }}</span>
        </div>
        <h3 class="award-title">{{ a.title }}</h3>
        <p class="award-detail">{{ a.detail }}</p>
        <div class="award-foot">
          <span class="award-place">{{ a.place }}</span>
        </div>
      </li>
    </ol>
  </div>
</template>

<style scoped>
.awards {
  margin-top: clamp(36px, 5vw, 52px);
}

.awards-list {
  list-style: none;
  margin: 0;
  padding: 0;
  display: grid;
  border: 1px solid var(--line);
  border-radius: var(--radius-lg);
  overflow: hidden;
  background: var(--line);
  gap: 1px;
}

.award {
  background: var(--ink-1);
  padding: 22px 24px;
  display: grid;
  gap: 12px;
  transition: background var(--dur-fast);
}

.award:hover {
  background: var(--ink-2);
}

.award-head {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 12px;
}

.award-rank {
  font-family: var(--font-mono);
  font-size: 11px;
  letter-spacing: 0.14em;
  text-transform: uppercase;
  color: var(--emerald);
}

.award-period {
  font-family: var(--font-mono);
  font-size: 11px;
  letter-spacing: 0.08em;
  color: var(--text-3);
  white-space: nowrap;
}

.award-title {
  font-family: var(--font-display);
  font-size: 17px;
  font-weight: 600;
  letter-spacing: 0;
  color: var(--text-0);
  margin: 0;
  line-height: 1.3;
}

.award-detail {
  margin: 0;
  font-size: 14px;
  line-height: 1.6;
  color: var(--text-2);
}

.award-foot {
  display: flex;
}

.award-place {
  font-family: var(--font-mono);
  font-size: 11px;
  letter-spacing: 0.08em;
  color: var(--text-3);
}
</style>
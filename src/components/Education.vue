<script setup lang="ts">
interface Degree {
  degree: string;
  school: string;
  place: string;
  period: string;
  note?: string;
}

interface Props {
  education: Degree[];
  languages: string;
}

defineProps<Props>();
</script>

<template>
  <div class="education reveal reveal--up">
    <div class="edu-grid">
      <div
        v-for="(e, i) in education"
        :key="e.degree"
        class="edu card"
        :style="{ '--d': i }"
      >
        <div class="edu-head">
          <span class="code-strip" aria-hidden="true">
            <span class="code-block code-block--dim code-block--cyan"></span>
          </span>
          <h3 class="edu-degree">{{ e.degree }}</h3>
          <span class="edu-note" v-if="e.note">{{ e.note }}</span>
        </div>
        <div class="edu-school">{{ e.school }}</div>
        <div class="edu-foot">
          <span class="edu-key">{{ e.place }}</span>
          <span class="edu-key">{{ e.period }}</span>
        </div>
      </div>

      <div class="edu-langs card" style="--d: 3">
        <div class="edu-head">
          <span class="code-strip" aria-hidden="true">
            <span class="code-block code-block--dim code-block--violet"></span>
          </span>
          <h3 class="edu-degree">Languages</h3>
        </div>
        <div class="edu-school">{{ languages }}</div>
        <div class="edu-foot">
          <span class="edu-key">for international teams</span>
          <span class="edu-key">ES · EN</span>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.education {
  margin-top: clamp(36px, 5vw, 52px);
}

.edu-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(260px, 1fr));
  gap: 1px;
  background: var(--line);
  border: 1px solid var(--line);
  border-radius: var(--radius-lg);
  overflow: hidden;
}

.card {
  background: var(--ink-1);
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  gap: 18px;
  padding: 20px 22px;
  position: relative;
  overflow: hidden;
  transition: background var(--dur-fast);
}

.card::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 2px;
  background: var(--cyan);
  opacity: 0.5;
  transition: height 0.2s var(--dur-fast), opacity 0.2s var(--dur-fast);
}

.card:hover {
  background: var(--ink-2);
}

.card:hover::before {
  height: 3px;
  opacity: 1;
}

.edu-langs::before {
  background: var(--violet);
}

.edu-head {
  display: grid;
  gap: 8px;
}

.edu-degree {
  font-family: var(--font-display);
  font-size: 15px;
  font-weight: 600;
  letter-spacing: 0;
  color: var(--text-0);
  margin: 0;
  line-height: 1.3;
}

.edu-note {
  font-family: var(--font-mono);
  font-size: 10px;
  letter-spacing: 0.14em;
  text-transform: uppercase;
  color: var(--emerald);
}

.edu-school {
  font-family: var(--font-mono);
  font-size: 12px;
  color: var(--text-2);
}

.edu-langs .edu-school {
  font-size: 13px;
  line-height: 1.6;
}

.edu-foot {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 10px;
}

.edu-key {
  font-family: var(--font-mono);
  font-size: 10.5px;
  letter-spacing: 0.08em;
  color: var(--text-3);
  white-space: nowrap;
}

@media (max-width: 640px) {
  .edu-grid {
    grid-template-columns: 1fr;
  }
}
</style>
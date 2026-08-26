<script setup lang="ts">
interface Certification {
  title: string;
  issuer: string;
  period: string;
  accent: 'emerald' | 'cyan' | 'violet';
}

interface Props {
  certifications: Certification[];
}

defineProps<Props>();

function slug(label: string): string {
  return label.toLowerCase().replace(/[^a-z0-9]+/g, '-');
}
</script>

<template>
  <div class="certs reveal reveal--up">
    <div
      v-for="(c, i) in certifications"
      :key="c.title"
      class="cert"
      :class="`cert--${c.accent}`"
      :style="{ '--d': i }"
    >
      <div class="cert-head">
        <span class="code-strip" aria-hidden="true">
          <span class="code-block code-block--dim" :class="`code-block--${c.accent}`"></span>
        </span>
        <h3 class="cert-title">{{ c.title }}</h3>
        <span class="cert-issuer">{{ c.issuer }}</span>
      </div>
      <div class="cert-foot">
        <span class="cert-path">{{ slug(c.title) }}.cert</span>
        <span class="cert-period">{{ c.period }}</span>
      </div>
    </div>
  </div>
</template>

<style scoped>
.certs {
  margin-top: clamp(36px, 5vw, 52px);
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 1px;
  background: var(--line);
  border: 1px solid var(--line);
  border-radius: var(--radius-lg);
  overflow: hidden;
}

.cert {
  background: var(--ink-1);
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  padding: 20px 22px;
  gap: 18px;
  position: relative;
  overflow: hidden;
  transition: background var(--dur-fast);
}

.cert::before {
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

.cert:hover {
  background: var(--ink-2);
}

.cert:hover::before {
  height: 3px;
  opacity: 1;
}

.cert--emerald { --accent: var(--emerald); }
.cert--cyan { --accent: var(--cyan); }
.cert--violet { --accent: var(--violet); }

.cert-head {
  display: grid;
  gap: 8px;
}

.cert-title {
  font-family: var(--font-display);
  font-size: 15px;
  font-weight: 600;
  letter-spacing: 0;
  color: var(--text-0);
  margin: 0;
  line-height: 1.3;
  transition: color var(--dur-fast);
}

.cert:hover .cert-title {
  color: var(--accent, var(--text-0));
}

.cert-issuer {
  font-family: var(--font-mono);
  font-size: 12px;
  color: var(--text-2);
  transition: color var(--dur-fast);
}

.cert:hover .cert-issuer {
  color: var(--text-0);
}

.cert-foot {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 10px;
}

.cert-path {
  font-family: var(--font-mono);
  font-size: 10.5px;
  letter-spacing: 0.08em;
  color: var(--text-3);
}

.cert-period {
  font-family: var(--font-mono);
  font-size: 10.5px;
  letter-spacing: 0.08em;
  color: var(--text-3);
  white-space: nowrap;
}

@media (max-width: 640px) {
  .certs {
    grid-template-columns: 1fr;
  }
}
</style>

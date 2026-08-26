<script setup lang="ts">
import { computed, ref } from 'vue';

interface Stage {
  id: string;
  number: string;
  stage: string;
  title: string;
  tools: string;
  detail: string;
  accent: 'emerald' | 'cyan' | 'violet';
  status: string;
}

const stages: Stage[] = [
  {
    id: 'source',
    number: '01',
    stage: 'Source',
    title: 'Multi-source & Microsoft Server',
    tools: 'Apache NiFi · REST APIs · SQL Server · Oracle · S3',
    detail: 'Concurrent Python extractors & NiFi processors ingest live Microsoft SQL Server databases, REST APIs, and SFTP drops into a raw immutable landing zone.',
    accent: 'emerald',
    status: 'STREAMING',
  },
  {
    id: 'ingest',
    number: '02',
    stage: 'Ingest',
    title: 'Orchestration Engine',
    tools: 'Airflow · Control-M · AWS Glue',
    detail: 'Airflow DAGs orchestrate lakehouse data flows, Control-M manages on-prem Hadoop jobs, and AWS Glue executes serverless ELT with automated retries and CloudWatch alerts.',
    accent: 'emerald',
    status: 'SCHEDULED',
  },
  {
    id: 'transform',
    number: '03',
    stage: 'Transform',
    title: 'Distributed Compute & Logic',
    tools: 'PySpark · Hadoop · Delta Lake',
    detail: 'Migrating legacy SQL and building custom transformations from scratch using deep business knowledge. Written in typed PySpark following PEP 8, with predefined schemas, table partitioning, and rigorous Pytest validation.',
    accent: 'cyan',
    status: 'OPTIMIZED',
  },
  {
    id: 'store',
    number: '04',
    stage: 'Store',
    title: 'Medallion Lakehouse',
    tools: 'Bronze → Silver → Gold · S3 · Azure Blob · Parquet',
    detail: 'Data flows through structured Medallion layers over AWS S3, Azure Blob Storage, and HDFS, ensuring pristine data governance, rapid partition pruning, and batch-load efficiency.',
    accent: 'cyan',
    status: 'SECURED',
  },
  {
    id: 'serve',
    number: '05',
    stage: 'Serve',
    title: 'BI & LLM / RAG Layer',
    tools: 'Power BI · Azure OpenAI · LangChain',
    detail: 'Gold-layer schemas power executive Power BI dashboards while Azure OpenAI + LangChain RAG pipelines ground AI answers in verified enterprise data.',
    accent: 'violet',
    status: 'CONNECTED',
  },
];

const active = ref<string>(stages[0].id);
const activeIndex = computed(() => stages.findIndex((s) => s.id === active.value));
const activeStage = computed(() => stages[activeIndex.value]);

function isActive(i: number): boolean {
  return i === activeIndex.value;
}
</script>

<template>
  <div class="pipeline-flow">
    <!-- Interactive Node Topology Bar -->
    <div class="topology-grid" role="tablist" aria-label="Data Pipeline Topology">
      <button
        v-for="(s, i) in stages"
        :key="s.id"
        class="node-card"
        :class="[
          `node-card--${s.accent}`,
          { 'node-card--active': isActive(i) },
        ]"
        role="tab"
        :aria-selected="isActive(i)"
        @click="active = s.id"
      >
        <div class="node-header">
          <span class="node-num">{{ s.number }}</span>
          <span class="node-status" :class="`status--${s.accent}`">
            <span class="status-dot"></span>
            {{ s.status }}
          </span>
        </div>
        <div class="node-body">
          <span class="node-stage">{{ s.stage }}</span>
          <h3 class="node-title">{{ s.title }}</h3>
        </div>
        <div class="node-glow" :class="`glow--${s.accent}`"></div>
      </button>
    </div>

    <!-- Active Stage Telemetry & Architecture Plate -->
    <div class="telemetry-plate" :class="`plate--${activeStage.accent}`">
      <Transition name="fade-slide" mode="out-in">
        <div class="plate-content" :key="activeStage.id">
          <div class="plate-top">
            <div class="plate-meta">
              <span class="datalabel" :class="`datalabel--${activeStage.accent}`">
                Stage {{ activeStage.number }} · {{ activeStage.stage.toUpperCase() }}
              </span>
              <h2 class="plate-heading">{{ activeStage.title }}</h2>
            </div>
            <div class="plate-badge" :class="`badge--${activeStage.accent}`">
              {{ activeStage.status }}
            </div>
          </div>

          <div class="plate-grid">
            <div class="plate-main">
              <p class="plate-desc">{{ activeStage.detail }}</p>
              <div class="plate-tools-wrap">
                <span class="tools-label">Stack & Tools:</span>
                <span class="tools-list">{{ activeStage.tools }}</span>
              </div>
            </div>
          </div>
        </div>
      </Transition>
    </div>
  </div>
</template>

<style scoped>
.pipeline-flow {
  display: grid;
  gap: 24px;
  margin-top: clamp(32px, 5vw, 48px);
}

/* Topology Grid */
.topology-grid {
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  gap: 12px;
}

@media (max-width: 900px) {
  .topology-grid {
    grid-template-columns: repeat(2, 1fr);
  }
}
@media (max-width: 540px) {
  .topology-grid {
    grid-template-columns: 1fr;
  }
}

.node-card {
  position: relative;
  background: var(--ink-1);
  border: 1px solid var(--line);
  border-radius: var(--radius-md);
  padding: 18px;
  text-align: left;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  gap: 12px;
  cursor: pointer;
  overflow: hidden;
  transition: all var(--dur-med) var(--ease-out);
}

.node-card:hover {
  background: var(--ink-2);
  border-color: var(--line-strong);
  transform: translateY(-2px);
  box-shadow: var(--shadow-soft);
}

.node-card--active {
  background: var(--ink-2);
  border-color: var(--line-accent);
  box-shadow: var(--shadow-medium);
}

.node-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 8px;
}

.node-num {
  font-family: var(--font-mono);
  font-size: 11px;
  color: var(--text-3);
  letter-spacing: 0.1em;
}

.node-status {
  display: flex;
  align-items: center;
  gap: 5px;
  font-family: var(--font-mono);
  font-size: 9.5px;
  letter-spacing: 0.08em;
  text-transform: uppercase;
}

.status-dot {
  width: 6px;
  height: 6px;
  border-radius: 50%;
  background: currentColor;
  box-shadow: 0 0 8px currentColor;
  animation: pulseDot 2s infinite ease-in-out;
}

@keyframes pulseDot {
  0%, 100% { opacity: 0.4; transform: scale(0.9); }
  50% { opacity: 1; transform: scale(1.2); }
}

.status--emerald { color: var(--emerald); }
.status--cyan { color: var(--cyan); }
.status--violet { color: var(--violet); }

.node-body {
  display: grid;
  gap: 4px;
}

.node-stage {
  font-family: var(--font-mono);
  font-size: 10px;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  color: var(--text-2);
}

.node-title {
  margin: 0;
  font-size: 14px;
  font-weight: 700;
  color: var(--text-0);
  line-height: 1.3;
}

.node-glow {
  position: absolute;
  top: 0;
  left: 0;
  width: 4px;
  height: 100%;
  opacity: 0.3;
  transition: opacity var(--dur-fast);
}

.node-card--active .node-glow,
.node-card:hover .node-glow {
  opacity: 1;
}

.glow--emerald { background: var(--emerald); box-shadow: 0 0 12px var(--emerald); }
.glow--cyan { background: var(--cyan); box-shadow: 0 0 12px var(--cyan); }
.glow--violet { background: var(--violet); box-shadow: 0 0 12px var(--violet); }

/* Telemetry Plate */
.telemetry-plate {
  background: var(--ink-1);
  border: 1px solid var(--line-strong);
  border-radius: var(--radius-plate);
  padding: 32px;
  position: relative;
  overflow: hidden;
}

.telemetry-plate::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 3px;
}

.plate--emerald::before { background: var(--emerald); box-shadow: 0 0 16px var(--emerald); }
.plate--cyan::before { background: var(--cyan); box-shadow: 0 0 16px var(--cyan); }
.plate--violet::before { background: var(--violet); box-shadow: 0 0 16px var(--violet); }

.plate-content {
  display: grid;
  gap: 20px;
}

.plate-top {
  display: flex;
  align-items: flex-start;
  justify-content: space-between;
  gap: 16px;
  flex-wrap: wrap;
}

.plate-meta {
  display: grid;
  gap: 6px;
}

.datalabel--emerald { color: var(--emerald); }
.datalabel--cyan { color: var(--cyan); }
.datalabel--violet { color: var(--violet); }

.plate-heading {
  margin: 0;
  font-size: clamp(20px, 3vw, 26px);
  font-weight: 800;
  color: var(--text-0);
  letter-spacing: -0.01em;
}

.plate-badge {
  font-family: var(--font-mono);
  font-size: 11px;
  font-weight: 600;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  padding: 6px 12px;
  border-radius: var(--radius-full);
  border: 1px solid currentColor;
}

.badge--emerald { color: var(--emerald); background: rgba(114, 176, 120, 0.1); }
.badge--cyan { color: var(--cyan); background: rgba(109, 182, 199, 0.1); }
.badge--violet { color: var(--violet); background: rgba(178, 133, 214, 0.1); }

.plate-grid {
  display: grid;
  grid-template-columns: 1fr;
  gap: 24px;
  align-items: center;
}

.plate-main {
  display: grid;
  gap: 14px;
}

.plate-desc {
  margin: 0;
  font-size: 16px;
  line-height: 1.7;
  color: var(--text-1);
  max-width: 75ch;
}

.plate-tools-wrap {
  display: flex;
  align-items: center;
  gap: 8px;
  flex-wrap: wrap;
  font-family: var(--font-mono);
  font-size: 12.5px;
}

.tools-label {
  color: var(--text-3);
  text-transform: uppercase;
  letter-spacing: 0.05em;
}

.tools-list {
  color: var(--text-0);
  background: var(--ink-2);
  padding: 4px 10px;
  border-radius: var(--radius-sm);
  border: 1px solid var(--line);
}

/* Transition */
.fade-slide-enter-active,
.fade-slide-leave-active {
  transition: opacity 0.2s var(--ease-out), transform 0.2s var(--ease-out);
}

.fade-slide-enter-from {
  opacity: 0;
  transform: translateY(8px);
}

.fade-slide-leave-to {
  opacity: 0;
  transform: translateY(-8px);
}
</style>

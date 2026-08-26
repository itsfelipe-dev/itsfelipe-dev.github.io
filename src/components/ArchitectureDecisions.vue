<script setup lang="ts">
import { ref } from 'vue';

interface Decision {
  title: string;
  context: string;
  decision: string;
  outcome: string;
  tradeoff?: string;
  accent: 'emerald' | 'cyan' | 'violet';
  icon: string;
}

const decisions: Decision[] = [
  {
    title: 'Impala over Hive for real-time API queries',
    context: 'NiFi-ingested streams needed to serve downstream APIs at 2B+ records/month. Hive’s batch-oriented MapReduce delivered 15-30s latency.',
    decision: 'Chose Apache Impala’s MPP architecture for sub-second query latency, enabling real-time decisioning instead of batch windows.',
    outcome: 'API response times reduced from 15-30s to <500ms.',
    tradeoff: 'Higher memory footprint; required tuning for production stability.',
    accent: 'emerald',
  },
  {
    title: 'Hudi tables over raw Parquet for lakehouse',
    context: 'Bronze/Silver layers needed ACID transactions and audit trails for regulated banking and pharma environments.',
    decision: 'Implemented Apache Hudi for upserts, time-travel, and data correction without full reprocessing.',
    outcome: 'Data quality incident resolution reduced from hours to minutes.',
    tradeoff: 'Added operational complexity; required team upskilling on Hudi APIs.',
    accent: 'cyan',
  },
  {
    title: '128MB block compaction strategy for HDFS',
    context: 'Small file problem causing NameNode memory exhaustion and degraded query performance.',
    decision: 'Enforced 128MB target file sizes via Spark compaction; avoided high-cardinality partitioning.',
    outcome: 'Query performance improved 3-5x by eliminating file listing overhead.',
    tradeoff: 'Increased write latency during compaction windows.',
    accent: 'cyan',
  },
  {
    title: 'YAML standardization for pipeline configuration',
    context: 'Config sprawl across JSON and properties files caused version control conflicts and review friction.',
    decision: 'Standardized on YAML with Jinja2 templating and schema validation for all pipeline configs.',
    outcome: 'Config-related incidents reduced by 40%; enabled non-engineers to review settings.',
    tradeoff: 'Required migration effort and validation infrastructure.',
    accent: 'violet',
  },
];

const hoverIndex = ref(-1);
</script>

<template>
  <div class="architecture reveal reveal--up">
    <div class="arch-intro">
      <p class="arch-lede">
        An engineer's call only counts when it ships. These are the ones
        I made - what each one cost, and what it came back with.
      </p>
    </div>

    <ol class="arch-ledger">
      <li
        v-for="(d, i) in decisions"
        :key="d.title"
        class="arch-entry"
        :class="[`arch-entry--${d.accent}`, { 'arch-entry--hover': hoverIndex === i }]"
        :style="{ '--d': i }"
        @mouseenter="hoverIndex = i"
        @mouseleave="hoverIndex = -1"
      >
        <div class="arch-main">
          <div class="arch-head">
            <h3 class="arch-title">{{ d.title }}</h3>
            <span class="arch-outcome" :class="`arch-outcome--${d.accent}`">{{ d.outcome }}</span>
          </div>

          <div class="arch-context">
            <span class="datalabel">context</span>
            <p>{{ d.context }}</p>
          </div>

          <div class="arch-decision">
            <span class="datalabel" :class="`datalabel--${d.accent}`">decision</span>
            <p>{{ d.decision }}</p>
          </div>

          <div v-if="d.tradeoff" class="arch-tradeoff">
            <span class="datalabel">tradeoff</span>
            <p>{{ d.tradeoff }}</p>
          </div>

          <div class="arch-foot">
            <span class="code-strip" aria-hidden="true">
              <span class="code-block" :class="`code-block--${d.accent}`"></span>
            </span>
            <span class="arch-layer">architectural decision</span>
          </div>
        </div>
      </li>
    </ol>
  </div>
</template>

<style scoped>
.architecture {
  display: grid;
  gap: clamp(28px, 4vw, 44px);
  margin-top: clamp(36px, 5vw, 52px);
}

.arch-intro {
  display: grid;
  gap: 12px;
}

.arch-lede {
  margin: 0;
  font-size: clamp(15px, 1.8vw, 18px);
  line-height: 1.65;
  color: var(--text-1);
  max-width: 680px;
}

.arch-lede strong {
  color: var(--text-0);
  font-weight: 700;
}

.arch-ledger {
  list-style: none;
  margin: 0;
  padding: 0;
  display: grid;
  gap: 16px;
}

.arch-entry {
  display: grid;
  gap: 0;
  background: var(--ink-1);
  border-radius: var(--radius-lg);
  border: 1px solid var(--line);
  transition: all var(--dur-fast) var(--ease-elastic);
  box-shadow: var(--shadow-soft);
  cursor: pointer;
}

.arch-entry:hover {
  transform: translateY(-4px);
  box-shadow: var(--shadow-medium);
  border-color: var(--line-strong);
}

.arch-entry--hover {
  transform: translateY(-6px);
  box-shadow: var(--shadow-medium);
  border-color: var(--accent, var(--line-strong));
  background: var(--ink-2);
}

.arch-entry--emerald:hover { --accent: var(--emerald); }
.arch-entry--cyan:hover { --accent: var(--cyan); }
.arch-entry--violet:hover { --accent: var(--violet); }

.arch-main {
  padding: 24px 22px;
  display: grid;
  gap: 16px;
}

.arch-head {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  gap: 16px;
  flex-wrap: wrap;
}

.arch-title {
  font-family: var(--font-display);
  font-size: clamp(17px, 2.2vw, 22px);
  font-weight: 700;
  letter-spacing: -0.01em;
  line-height: 1.3;
  color: var(--text-0);
  margin: 0;
  transition: color var(--dur-fast);
}

.arch-entry:hover .arch-title {
  color: var(--accent, var(--text-0));
}

.arch-outcome {
  font-family: var(--font-display);
  font-size: 12px;
  font-weight: 700;
  letter-spacing: 0.02em;
  white-space: nowrap;
  padding: 6px 14px;
  border-radius: var(--radius-full);
  background: var(--ink-3);
  border: 1px solid var(--line-strong);
}

.arch-outcome--emerald { color: var(--emerald); border-color: var(--emerald); background: rgba(107, 142, 107, 0.1); }
.arch-outcome--cyan { color: var(--cyan); border-color: var(--cyan); background: rgba(107, 142, 158, 0.1); }
.arch-outcome--violet { color: var(--violet); border-color: var(--violet); background: rgba(142, 107, 158, 0.1); }

.arch-context,
.arch-decision,
.arch-tradeoff {
  display: grid;
  gap: 6px;
}

.arch-context p,
.arch-decision p,
.arch-tradeoff p {
  margin: 0;
  font-size: 14px;
  line-height: 1.6;
  color: var(--text-1);
  max-width: 90ch;
}

.arch-context {
  padding-top: 8px;
  border-top: 1px solid var(--line);
}

.arch-decision {
  padding-top: 8px;
  border-top: 1px solid var(--line);
}

.arch-tradeoff {
  padding-top: 8px;
  border-top: 1px solid var(--line);
  background: var(--ink-2);
  border-radius: var(--radius-md);
  padding-bottom: 8px;
}

.arch-tradeoff p {
  color: var(--text-2);
}

.arch-wisdom {
  display: flex;
  align-items: center;
  gap: 8px;
  margin-top: 8px;
  padding: 6px 12px;
  background: var(--ink-2);
  border-radius: var(--radius-md);
  font-size: 13px;
  color: var(--text-1);
  font-style: italic;
  border: 1px solid var(--line);
  animation: wisdomFadeIn 0.3s var(--ease-out);
}

@keyframes wisdomFadeIn {
  from {
    opacity: 0;
    transform: translateY(-4px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.wisdom-icon {
  font-size: 16px;
}

.wisdom-text {
  font-family: var(--font-display);
  letter-spacing: -0.01em;
}

.datalabel--emerald { color: var(--emerald); }
.datalabel--cyan { color: var(--cyan); }
.datalabel--violet { color: var(--violet); }

.arch-foot {
  display: flex;
  align-items: center;
  gap: 14px;
  padding-top: 12px;
  border-top: 1px solid var(--line);
}

.arch-layer {
  margin-left: auto;
  font-family: var(--font-mono);
  font-size: 10.5px;
  letter-spacing: 0.08em;
  color: var(--text-3);
}

@media (max-width: 640px) {
  .arch-head {
    flex-direction: column;
  }

  .arch-outcome {
    align-self: flex-start;
    white-space: normal;
    text-wrap: balance;
  }

  .arch-wisdom {
    justify-content: center;
    text-align: center;
  }
}
</style>
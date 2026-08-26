<script setup lang="ts">
import { computed, ref } from 'vue';

interface SocialLink {
  label: string;
  href: string;
  value: string;
  icon: string;
}

interface Props {
  email: string;
  phone?: string;
  links: SocialLink[];
}

const props = defineProps<Props>();

const name = ref('');
const company = ref('');
const topic = ref('Data engineering');
const message = ref('');

const topics = ['Data engineering', 'Cloud migration', 'AI / RAG consulting', 'Other'];

const mailtoHref = computed(() => {
  const subject = encodeURIComponent(`[${topic.value}] ${name.value ? name.value : 'Contact'} - portfolio inquiry`);
  const body = encodeURIComponent(
    `${message.value}\n\n-\n${name.value}${company.value ? `, ${company.value}` : ''}`,
  );
  return `mailto:${props.email}?subject=${subject}&body=${body}`;
});

const isReady = computed(() => message.value.trim().length > 0);
</script>

<template>
  <div class="contact-grid reveal reveal--up">
    <form class="cform" action="." method="get" @submit.prevent>
      <div class="cform-head">
        <p class="datalabel">start a conversation</p>
        <h3 class="cform-title">Send a message</h3>
        <p class="cform-sub">
          Tell me what you're building. I reply from
          <a :href="`mailto:${email}`" class="cform-mail">{{ email }}</a>
          within a day or two.
        </p>
      </div>

      <div class="cform-fields">
        <label class="field">
          <span class="datalabel">your name</span>
          <input v-model="name" type="text" placeholder="Jane Doe" class="field-input" />
        </label>
        <label class="field">
          <span class="datalabel">company</span>
          <input v-model="company" type="text" placeholder="Acme Analytics" class="field-input" />
        </label>
        <label class="field">
          <span class="datalabel">topic</span>
          <select v-model="topic" class="field-input field-select">
            <option v-for="t in topics" :key="t" :value="t">{{ t }}</option>
          </select>
        </label>
        <label class="field field--full">
          <span class="datalabel">message</span>
          <textarea
            v-model="message"
            rows="4"
            placeholder="We're migrating legacy SQL to PySpark and need…"
            class="field-input field-textarea"
          ></textarea>
        </label>
      </div>

      <div class="cform-foot">
        <a
          :href="mailtoHref"
          class="btn btn-primary"
          :class="{ 'btn--disabled': !isReady }"
          :aria-disabled="!isReady"
        >
          <svg width="12" height="12" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true">
            <line x1="22" y1="2" x2="11" y2="13" />
            <polygon points="22 2 15 22 11 13 2 9 22 2" />
          </svg>
          <span>Send message</span>
        </a>
      </div>
    </form>

    <aside class="cindex">
      <div class="cindex-head">
        <p class="datalabel">direct channels</p>
        <h3 class="cindex-title">Direct channels</h3>
      </div>

      <div class="cindex-rows">
        <a class="cindex-row" :href="`mailto:${email}`">
          <span class="cindex-key">email</span>
          <span class="cindex-val">{{ email }}</span>
        </a>
        <a v-if="phone" class="cindex-row" :href="`tel:${phone.replace(/[^+\d]/g, '')}`">
          <span class="cindex-key">phone</span>
          <span class="cindex-val">{{ phone }}</span>
        </a>
        <a v-for="l in links" :key="l.label" class="cindex-row" :href="l.href" target="_blank" rel="noopener">
          <span class="cindex-key">{{ l.label.toLowerCase() }}</span>
          <span class="cindex-val">{{ l.value }}</span>
        </a>
      </div>


    </aside>
  </div>
</template>

<style scoped>
.contact-grid {
  margin-top: clamp(36px, 5vw, 52px);
  display: grid;
  grid-template-columns: 1.4fr 1fr;
  gap: 1px;
  background: var(--line);
  border: 1px solid var(--line);
  border-radius: var(--radius-plate);
  overflow: hidden;
  align-items: stretch;
}

.cform,
.cindex {
  background: var(--ink-1);
  display: flex;
  flex-direction: column;
}

.cform {
  padding: clamp(22px, 3.5vw, 32px);
  gap: 22px;
}

.cindex {
  padding: clamp(22px, 3.5vw, 32px);
  gap: 22px;
}

.cform-head,
.cindex-head {
  display: grid;
  gap: 8px;
}

.cform-title,
.cindex-title {
  font-family: var(--font-display);
  font-size: clamp(18px, 2.4vw, 24px);
  font-weight: 600;
  letter-spacing: 0.02em;
  color: var(--text-0);
  margin: 0;
  line-height: 1.3;
}

.cform-sub {
  margin: 0;
  font-size: 13.5px;
  color: var(--text-2);
  line-height: 1.6;
}

.cform-mail {
  color: var(--text-0);
  text-decoration: none;
  border-bottom: 1px solid var(--line-accent);
}

.cform-fields {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 16px;
}

.field {
  display: grid;
  gap: 7px;
}

.field--full {
  grid-column: 1 / -1;
}

.field-input {
  font-family: var(--font-body);
  font-size: 14px;
  color: var(--text-0);
  background: var(--ink-0);
  border: 1px solid var(--line);
  border-radius: var(--radius-plate);
  padding: 11px 13px;
  transition: border-color var(--dur-fast);
  outline: none;
  width: 100%;
}

.field-input::placeholder {
  color: var(--text-3);
}

.field-input:focus {
  border-color: var(--line-accent);
}

.field-select {
  appearance: none;
  background-image: linear-gradient(45deg, transparent 50%, var(--text-3) 50%), linear-gradient(135deg, var(--text-3) 50%, transparent 50%);
  background-position: calc(100% - 18px) 50%, calc(100% - 13px) 50%;
  background-size: 5px 5px;
  background-repeat: no-repeat;
  cursor: pointer;
}

.field-textarea {
  resize: vertical;
  min-height: 108px;
}

.cform-foot {
  display: flex;
  align-items: center;
  gap: 14px;
  flex-wrap: wrap;
}

.btn--disabled {
  opacity: 0.5;
  pointer-events: none;
}

.cform-hint {
  margin: 0;
  font-family: var(--font-mono);
  font-size: 11px;
  color: var(--text-3);
}

.cindex-rows {
  display: grid;
  border: 1px solid var(--line);
  border-radius: var(--radius-plate);
  overflow: hidden;
}

.cindex-row {
  display: flex;
  align-items: center;
  gap: 14px;
  padding: 13px 16px;
  background: var(--ink-0);
  text-decoration: none;
  transition: background var(--dur-fast);
}

.cindex-row + .cindex-row {
  border-top: 1px solid var(--line);
}

.cindex-row:hover {
  background: var(--ink-2);
}

.cindex-key {
  font-family: var(--font-mono);
  font-size: 11px;
  font-weight: 500;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: var(--text-3);
  flex-shrink: 0;
  width: 62px;
}

.cindex-val {
  font-family: var(--font-mono);
  font-size: 13px;
  color: var(--text-0);
  min-width: 0;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.cindex-status {
  margin-top: auto;
  display: flex;
  align-items: center;
  gap: 10px;
}

@media (max-width: 720px) {
  .contact-grid {
    grid-template-columns: 1fr;
  }

  .cform-fields {
    grid-template-columns: 1fr;
  }
}
</style>

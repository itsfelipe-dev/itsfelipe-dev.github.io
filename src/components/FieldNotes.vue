<script setup lang="ts">
interface Article {
  title: string;
  date: string;
  tag: string;
  href: string;
}

const MEDIUM_URL = 'https://medium.com/@itsfelipe';

const articles: Article[] = [
  {
    title:
      'I Turned My Kindle Highlights Into Daily Inspiration - And Built a Tool So You Can Too',
    date: 'May 2025',
    tag: 'tooling',
    href: 'https://itsfelipe.medium.com/i-turned-my-kindle-highlights-into-daily-inspiration-and-built-a-tool-so-you-can-too-0d362a7ee6ea',
  },
  {
    title: 'Obtén las herramientas de JetBrains GRATIS con tu correo @soy.sena.edu.co',
    date: 'Apr 2025',
    tag: 'community',
    href: 'https://itsfelipe.medium.com/obt%C3%A9n-las-herramientas-de-jetbrains-gratis-con-tu-correo-soy-sena-edu-co-8047e94043f3',
  },
  {
    title: 'Why Every Programmer Should Try Linux: Lessons from 4+ Years of Experience',
    date: 'Jan 2025',
    tag: 'linux',
    href: 'https://itsfelipe.medium.com/why-every-programmer-should-try-linux-lessons-from-4-years-of-experience-50e1ab522de8',
  },
  {
    title: 'Emotion Analysis with Python and Google Gemini AI',
    date: 'Mar 2024',
    tag: 'ai · nlp',
    href: 'https://itsfelipe.medium.com/unlocking-workplace-harmony-geminis-python-and-streamlit-magic-for-emotion-driven-email-analysis-6592a8bcf2a2',
  },
  {
    title: 'Tutorial: Nifi Upload Files to SharePoint API with Template',
    date: 'Feb 2023',
    tag: 'integration',
    href: 'https://itsfelipe.medium.com/tutorial-nifi-upload-files-to-sharepoint-api-with-template-ae9fa94b3a79',
  },
];
</script>

<template>
  <div class="notes reveal reveal--up">
    <ol class="notes-list">
      <li v-for="(a, i) in articles" :key="a.href" class="note">
        <a :href="a.href" target="_blank" rel="noopener noreferrer" class="note-link">
          <span class="note-no">{{ String(i + 1).padStart(2, '0') }}</span>
          <span class="note-title">{{ a.title }}</span>
          <span class="note-meta">
            <span class="note-date">{{ a.date }}</span>
            <span class="note-tag">{{ a.tag }}</span>
          </span>
          <svg
            class="note-arrow"
            width="14"
            height="14"
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            stroke-width="2"
            stroke-linecap="round"
            stroke-linejoin="round"
            aria-hidden="true"
          >
            <line x1="7" y1="17" x2="17" y2="7" />
            <polyline points="7 7 17 7 17 17" />
          </svg>
        </a>
      </li>
    </ol>

    <div class="notes-foot">
      <span class="datalabel">source · medium</span>
      <a :href="MEDIUM_URL" target="_blank" rel="noopener noreferrer" class="notes-more">
        Read all notes on Medium
        <svg
          width="14"
          height="14"
          viewBox="0 0 24 24"
          fill="none"
          stroke="currentColor"
          stroke-width="2"
          stroke-linecap="round"
          stroke-linejoin="round"
          aria-hidden="true"
        >
          <line x1="7" y1="17" x2="17" y2="7" />
          <polyline points="7 7 17 7 17 17" />
        </svg>
      </a>
    </div>
  </div>
</template>

<style scoped>
.notes {
  margin-top: clamp(36px, 5vw, 52px);
  display: grid;
  gap: clamp(24px, 4vw, 36px);
}

.notes-list {
  list-style: none;
  margin: 0;
  padding: 0;
  background: var(--line);
  border: 1px solid var(--line);
  border-radius: var(--radius-lg);
  overflow: hidden;
  display: grid;
  gap: 1px;
}

.note {
  margin: 0;
  background: var(--ink-1);
  transition: background var(--dur-fast);
}

.note:hover {
  background: var(--ink-2);
}

.note-link {
  display: grid;
  grid-template-columns: auto 1fr auto auto;
  align-items: center;
  gap: 18px;
  padding: 18px 22px;
}

.note-no {
  font-family: var(--font-mono);
  font-size: 12px;
  color: var(--text-3);
  transition: color var(--dur-fast);
}

.note:hover .note-no {
  color: var(--accent-warm);
}

.note-title {
  font-family: var(--font-display);
  font-size: 15px;
  font-weight: 600;
  color: var(--text-0);
  line-height: 1.4;
  min-width: 0;
  transition: color var(--dur-fast);
}

.note:hover .note-title {
  color: var(--accent-warm);
}

.note-meta {
  display: flex;
  align-items: center;
  gap: 10px;
  min-width: 150px;
  justify-content: flex-end;
}

.note-date {
  font-family: var(--font-mono);
  font-size: 10.5px;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: var(--text-3);
  white-space: nowrap;
}

.note-tag {
  font-family: var(--font-mono);
  font-size: 9.5px;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  color: var(--text-3);
  border: 1px solid var(--line);
  border-radius: var(--radius-full);
  padding: 3px 9px;
  white-space: nowrap;
}

.note-arrow {
  color: var(--text-3);
  flex-shrink: 0;
  transition: transform var(--dur-fast) var(--ease-out), color var(--dur-fast);
}

.note:hover .note-arrow {
  color: var(--accent-warm);
  transform: translate(2px, -2px);
}

.notes-foot {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 16px;
  flex-wrap: wrap;
  padding-top: 4px;
}

.notes-more {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  font-family: var(--font-mono);
  font-size: 12px;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: var(--text-0);
  transition: color var(--dur-fast);
}

.notes-more svg {
  transition: transform var(--dur-fast) var(--ease-out);
}

.notes-more:hover {
  color: var(--accent-warm);
}

.notes-more:hover svg {
  transform: translate(2px, -2px);
}

@media (max-width: 760px) {
  .note-link {
    grid-template-columns: auto 1fr auto;
    grid-template-areas:
      'no title arrow'
      'no meta arrow';
    gap: 4px 12px;
    padding: 16px;
  }

  .note-no { grid-area: no; }
  .note-title { grid-area: title; }
  .note-meta { grid-area: meta; justify-content: flex-start; min-width: 0; }
  .note-arrow { grid-area: arrow; }
}
</style>

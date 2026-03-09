<template>
  <div class="app">

    <header class="header">
      <h1 class="logo">moodlog</h1>
      <span class="tagline">um registro simples do seu dia<br>projeto por Lucas Bonavina</span>
    </header>

    <main class="main">

      <section class="form-card">
        <div class="form-label">como você está hoje?</div>

        <MoodPicker v-model="form.mood" />

        <textarea
          v-model="form.note"
          class="note-input"
          placeholder="alguma nota… (opcional)"
          rows="3"
          maxlength="280"
        />

        <div class="form-footer">
          <span class="char-count">{{ form.note.length }}/280</span>
          <button
            class="btn-save"
            :disabled="!form.mood"
            @click="save"
          >
            registrar
          </button>
        </div>
      </section>

      <section class="list" v-if="entries.length">
        <div class="list-header">
          <span class="list-title">histórico</span>
          <button class="btn-clear" @click="clearAll">limpar tudo</button>
        </div>

        <EntryCard
          v-for="entry in entries"
          :key="entry.id"
          :entry="entry"
          @delete="remove"
        />
      </section>

      <div class="empty" v-else>
        <span class="empty-icon">◌</span>
        <p>nenhum registro ainda.<br>comece registrando seu humor acima.</p>
      </div>

    </main>

  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import MoodPicker from './components/MoodPicker.vue'
import EntryCard  from './components/EntryCard.vue'

const STORAGE_KEY = 'moodlog_entries'

const loadEntries = () => {
  try {
    return JSON.parse(localStorage.getItem(STORAGE_KEY)) || []
  } catch {
    return []
  }
}

const entries = ref(loadEntries())

const form = ref({ mood: '', note: '' })

function save() {
  if (!form.value.mood) return

  const entry = {
    id:   Date.now(),
    date: new Date().toISOString(),
    mood: form.value.mood,
    note: form.value.note.trim(),
  }

  entries.value.unshift(entry)
  persist()

  form.value.mood = ''
  form.value.note = ''
}

function remove(id) {
  entries.value = entries.value.filter(e => e.id !== id)
  persist()
}

function clearAll() {
  if (!confirm('Remover todos os registros?')) return
  entries.value = []
  persist()
}

function persist() {
  localStorage.setItem(STORAGE_KEY, JSON.stringify(entries.value))
}

const moodEmojis = { great:'✦', good:'◎', ok:'◌', bad:'◍', awful:'✕' }

</script>

<style scoped>
.app {
  max-width: 480px;
  margin: 0 auto;
  padding: 48px 20px 80px;
}

/* ── Header ── */
.header {
  margin-bottom: 40px;
}

.logo {
  font-family: 'DM Serif Display', serif;
  font-size: 32px;
  font-weight: 400;
  letter-spacing: -.5px;
  color: var(--ink);
  line-height: 1;
  margin-bottom: 4px;
}

.tagline {
  font-size: 11px;
  color: var(--ink3);
  letter-spacing: .06em;
}

/* ── Form card ── */
.form-card {
  background: var(--surface);
  border: 1.5px solid var(--border);
  border-radius: 14px;
  padding: 22px;
  display: flex;
  flex-direction: column;
  gap: 16px;
  margin-bottom: 16px;
}

.form-label {
  font-size: 11px;
  letter-spacing: .08em;
  text-transform: uppercase;
  color: var(--ink2);
}

.note-input {
  resize: none;
  background: var(--bg);
  border: 1.5px solid var(--border);
  border-radius: var(--radius);
  padding: 10px 12px;
  font-size: 13px;
  color: var(--ink);
  line-height: 1.6;
  outline: none;
  transition: border-color .15s;
  width: 100%;
}

.note-input::placeholder { color: var(--ink3); }
.note-input:focus { border-color: var(--ink); }

.form-footer {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.char-count {
  font-size: 10px;
  color: var(--ink3);
}

.btn-save {
  background: var(--ink);
  color: var(--bg);
  font-size: 11px;
  letter-spacing: .08em;
  text-transform: uppercase;
  padding: 9px 20px;
  border-radius: 99px;
  font-weight: 500;
  transition: background .15s, opacity .15s;
}

.btn-save:hover   { background: var(--accent); }
.btn-save:disabled { opacity: .3; cursor: not-allowed; }

/* ── Stats ── */
.stats {
  display: flex;
  align-items: center;
  gap: 16px;
  padding: 16px 22px;
  background: var(--surface);
  border: 1.5px solid var(--border);
  border-radius: 14px;
  margin-bottom: 16px;
}

.stat {
  display: flex;
  flex-direction: column;
  gap: 2px;
}

.stat-n {
  font-size: 18px;
  font-family: 'DM Serif Display', serif;
  color: var(--ink);
  line-height: 1;
}

.stat-l {
  font-size: 9px;
  text-transform: uppercase;
  letter-spacing: .06em;
  color: var(--ink3);
}

.stat-sep {
  width: 1px;
  height: 28px;
  background: var(--border);
}

/* ── List ── */
.list {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.list-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 4px;
}

.list-title {
  font-size: 11px;
  text-transform: uppercase;
  letter-spacing: .08em;
  color: var(--ink3);
}

.btn-clear {
  font-size: 10px;
  color: var(--ink3);
  letter-spacing: .04em;
  transition: color .15s;
}

.btn-clear:hover { color: var(--accent); }

/* ── Empty ── */
.empty {
  text-align: center;
  padding: 48px 0;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 12px;
}

.empty-icon {
  font-size: 28px;
  color: var(--ink3);
}

.empty p {
  font-size: 13px;
  color: var(--ink3);
  line-height: 1.7;
}
</style>

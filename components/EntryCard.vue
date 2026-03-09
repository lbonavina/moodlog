<template>
  <div class="entry" :class="entry.mood">
    <div class="entry-top">
      <span class="entry-mood">{{ moodMap[entry.mood]?.emoji }}</span>
      <span class="entry-date">{{ formattedDate }}</span>
      <button class="entry-delete" @click="$emit('delete', entry.id)" title="remover">×</button>
    </div>
    <p v-if="entry.note" class="entry-note">{{ entry.note }}</p>
  </div>
</template>

<script setup>
import { computed } from 'vue'

const props = defineProps({ entry: Object })
defineEmits(['delete'])

const moodMap = {
  great: { emoji: '✦' },
  good:  { emoji: '◎' },
  ok:    { emoji: '◌' },
  bad:   { emoji: '◍' },
  awful: { emoji: '✕' },
}

const formattedDate = computed(() => {
  const d = new Date(props.entry.date)
  return d.toLocaleDateString('pt-BR', { weekday: 'short', day: '2-digit', month: 'short' })
})
</script>

<style scoped>
.entry {
  padding: 14px 16px;
  border-radius: var(--radius);
  border: 1.5px solid var(--border);
  background: var(--surface);
  transition: border-color .15s;
  animation: slideIn .2s ease;
}

@keyframes slideIn {
  from { opacity: 0; transform: translateY(6px); }
  to   { opacity: 1; transform: translateY(0); }
}

.entry:hover { border-color: var(--ink3); }

.entry-top {
  display: flex;
  align-items: center;
  gap: 10px;
}

.entry-mood {
  font-size: 16px;
  line-height: 1;
  width: 20px;
  text-align: center;
}

/* accent color per mood */
.entry.great .entry-mood { color: #C9622F; }
.entry.good  .entry-mood { color: #6B8F71; }
.entry.ok    .entry-mood { color: #8B7355; }
.entry.bad   .entry-mood { color: #8B5A5A; }
.entry.awful .entry-mood { color: #4A4A4A; }

.entry-date {
  font-size: 11px;
  color: var(--ink2);
  letter-spacing: .04em;
  flex: 1;
}

.entry-delete {
  font-size: 16px;
  color: var(--ink3);
  line-height: 1;
  padding: 2px 4px;
  border-radius: 4px;
  transition: color .15s, background .15s;
  opacity: 0;
  transition: opacity .15s;
}

.entry:hover .entry-delete { opacity: 1; }
.entry-delete:hover { color: var(--accent); }

.entry-note {
  margin-top: 8px;
  font-size: 12.5px;
  color: var(--ink2);
  line-height: 1.6;
  padding-top: 8px;
  border-top: 1px solid var(--border);
}
</style>

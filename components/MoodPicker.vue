<template>
  <div class="picker">
    <button
      v-for="m in moods"
      :key="m.value"
      class="mood-btn"
      :class="{ selected: modelValue === m.value }"
      :title="m.label"
      @click="$emit('update:modelValue', m.value)"
    >
      <span class="emoji">{{ m.emoji }}</span>
      <span class="label">{{ m.label }}</span>
    </button>
  </div>
</template>

<script setup>
defineProps({ modelValue: String })
defineEmits(['update:modelValue'])

const moods = [
  { value: 'great',  emoji: '✦', label: 'ótimo'     },
  { value: 'good',   emoji: '◎', label: 'bem'        },
  { value: 'ok',     emoji: '◌', label: 'ok'         },
  { value: 'bad',    emoji: '◍', label: 'mal'        },
  { value: 'awful',  emoji: '✕', label: 'péssimo'    },
]
</script>

<style scoped>
.picker {
  display: flex;
  gap: 8px;
}

.mood-btn {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 5px;
  padding: 12px 14px;
  border-radius: var(--radius);
  border: 1.5px solid var(--border);
  background: var(--surface);
  color: var(--ink2);
  transition: border-color .15s, color .15s, background .15s;
  flex: 1;
}

.mood-btn:hover {
  border-color: var(--ink);
  color: var(--ink);
}

.mood-btn.selected {
  border-color: var(--accent);
  background: var(--bg);
  color: var(--accent);
}

.emoji {
  font-size: 18px;
  line-height: 1;
  font-style: normal;
}

.label {
  font-size: 9px;
  letter-spacing: .06em;
  text-transform: uppercase;
  font-weight: 500;
}
</style>

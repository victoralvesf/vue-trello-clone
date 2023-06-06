<template>
  <div
    :title="new Date(task.createdAt).toLocaleString()"
    class="task bg-white p-2 mb-2 rounded shadow-sm max-w-[250px] block overflow-auto"
    @focus="focused = true"
    @blur="focused = false"
    tabindex="0"
  >
    <DragHandle class="pr-2" icon-type="task" />
    <span
      class="custom-task-text"
      role="textbox"
      contenteditable
      @input="handleSpanChange($event)"
      @blur="handleSpanBlur($event)"
    >
      {{ task.title }}
    </span>
  </div>
</template>

<script setup lang="ts">
import type { ID, Task } from '@/types'

const props = defineProps<{
  task: Task,
}>()

const emit = defineEmits<{
  (e: 'delete', payload: ID): void
}>()

const focused = ref(false)

onKeyStroke('Backspace', (e) => {
  if (focused.value) emit('delete', props.task.id)
})

function handleSpanChange(event: Event) {
  const innerText = (event.target as HTMLElement).innerText

  props.task.title = innerText
}

function handleSpanBlur(event: Event) {
  const innerText = (event.target as HTMLElement).innerText
  
  if (innerText === '') {
    emit('delete', props.task.id)
  }
}
</script>

<style lang="css">
.custom-task-text {
  max-width: calc(100% - 22px);
  min-width: calc(100% - 22px);
  max-height: 6rem;
  word-wrap: break-word;
  padding: 0px 4px;
  display: block;
  overflow-y: auto;
  float: right;
}

.custom-task-text::-webkit-scrollbar {
  width: 6px;
  height: 6px;
}

.custom-task-text::-webkit-scrollbar-track {
  background: #f1f1f1;
  border-radius: 6px;
}

.custom-task-text::-webkit-scrollbar-thumb {
  background: #a9abad;
  border-radius: 6px;
}

.custom-task-text::-webkit-scrollbar-thumb:hover {
  background: #6b7280;
}
</style>
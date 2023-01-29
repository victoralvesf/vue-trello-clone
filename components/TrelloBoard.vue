<template>
  <section>
    <NewColumnButton @click="createColumn" class="block ml-auto" />
    <draggable
      v-model="columns"
      group="columns"
      :animation="250"
      handle=".drag-handle"
      item-key="id"
      :class="`section flex gap-4 items-start overflow-x-auto rounded ${scrollBar.join(' ')}`"
      v-if="columns.length > 0"
    >
      <template #item="{ element: column } : { element: Column }">
        <div class="column bg-gray-200 p-5 rounded min-w-[250px] max-w-[250px] shadow-sm transition-all duration-150 ease-out">
          <header class="mb-4 font-bold text-gray-800">
            <DragHandle icon-type="column" />
            <input
              type="text"
              class="title bg-transparent focus:bg-white outline-gray-400 rounded px-1 w-4/5"
              @keyup.enter="handleEnterKey($event, column)"
              @keydown.backspace="removeColumn(column)"
              v-model="column.title"
            >
          </header>
          <draggable
            v-model="column.tasks"
            :group="{ name: 'tasks', pull: alt ? 'clone' : true }"
            handle=".drag-handle"
            :animation="250"
            item-key="id"
          >
            <template #item="{ element: task } : { element: Task }">
              <div>
                <BoardTask :task="task" @delete="column.tasks = column.tasks.filter(t => t.id !== $event)"/>
              </div>
            </template>
          </draggable>
          <footer>
            <NewTask @add="column.tasks.push($event)" />
          </footer>
        </div>
      </template>
    </draggable>
    <NoColumns v-else @new-column="createColumn" />
  </section>
</template>

<script setup lang="ts">
import draggable from 'vuedraggable'
import { nanoid } from 'nanoid'
import { Column, Task } from "@/types";

const alt = useKeyModifier('Alt')

const scrollBar = [
  'scrollbar-thin',
  'scrollbar-thumb-gray-400',
  'hover:scrollbar-thumb-gray-500',
  'scrollbar-track-gray-300',
  'scrollbar-thumb-rounded-full',
  'scrollbar-track-rounded-full'
]

const columns = useLocalStorage<Column[]>('trelloBoard', [
  {
    id: nanoid(),
    title: 'Backlog',
    tasks: [
      {
        id: nanoid(),
        title: 'Your first task',
        createdAt: new Date()
      }
    ]
  },
])

function createColumn() {
  const column: Column = {
    id: nanoid(),
    title: '',
    tasks: []
  }
  columns.value.push(column)
  nextTick(() => {
    (document.querySelector('.column:last-of-type .title') as HTMLInputElement).focus()
  })
}

function removeColumn(column: Column) {
  if (column.title === '') {
    columns.value = columns.value.filter((c => c.id !== column.id))
  }
}

function handleEnterKey(event: KeyboardEvent, column: Column) {
  if (column.title === '') {
    columns.value = columns.value.filter((c => c.id !== column.id))
  } else {
    (event.target as HTMLInputElement).blur()
  }
}
</script>

<style lang="css">
  .section {
    height: calc(100vh - 42.55px - 11rem);
    max-height: calc(100vh - 42.55px - 11rem);
  }
</style>

<style lang="css">
.sortable-drag .task {
  transform: rotate(5deg);
}
.sortable-ghost .task {
  position: relative;
}
.sortable-ghost .task::after {
  content: "";
  @apply absolute top-0 bottom-0 left-0 right-0 bg-slate-300 rounded;
}

.task:focus,
.task:focus-visible {
  @apply outline-gray-400 !important;
  outline: gray auto 1px;
}
</style>

<script setup lang="ts">
import type { Task } from "@/types";
import { nanoid } from "nanoid";

const emit = defineEmits<{
  (e: "add", payload: Task): void;
}>();

const focused = ref(false);
const title = ref("");

function createTask(e: Event) {
  if (title.value.trim()) {
    e.preventDefault();
    emit("add", {
      title: title.value.trim(),
      createdAt: new Date(),
      id: nanoid(),
    } as Task);
  }

  title.value = "";
}
</script>
<template>
  <div
    :class="{
      'h-10': !focused,
      'h-20': focused
    }"
  >
    <textarea
      v-model="title"
      @keydown.tab="createTask"
      @keyup.enter="createTask"
      class="focus:bg-white focus:shadow resize-none rounded w-full border-none p-2 bg-transparent cursor-pointer transition-all duration-150 ease-out"
      :class="{
        'h-10 hover:bg-gray-300 placeholder:text-gray-600': !focused,
        'h-20': focused,
      }"
      style="outline: none !important"
      @focus="focused = true"
      @blur="focused = false"
      :placeholder="!focused ? '+ Add a card' : 'Enter a title for this card'"
    />
  </div>
</template>
<template>
  <span class="drag-handle text-gray-500 mr-1 cursor-grab">
    <font-awesome-icon v-if="iconType === 'column'" icon="fa-solid fa-grip-vertical" class="mx-2" />
    <font-awesome-icon v-else icon="fa-solid fa-grip" />
  </span>
</template>

<script setup lang="ts">
import { DragHandleStyle } from '~~/types';

defineProps<{
  iconType?: DragHandleStyle,
}>()
</script>

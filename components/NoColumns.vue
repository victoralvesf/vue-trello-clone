<template>
  <div class="no-columns-message flex flex-col items-center justify-center">
    <h3 class="text-2xl mb-6 text-gray-200">
      You don't have any tasks!
      <font-awesome-icon icon="fa-solid fa-wand-magic-sparkles" />
    </h3>
    <NewColumnButton @click="$emit('newColumn')" />
  </div>
</template>

<style lang="css">
  .no-columns-message {
    height: calc(100vh - 42.55px - 11rem);
    max-height: calc(100vh - 42.55px - 11rem);
  }
</style>
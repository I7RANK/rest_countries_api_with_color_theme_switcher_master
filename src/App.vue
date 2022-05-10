<script setup>
import { RouterView } from "vue-router";
</script>

<template>
  <header class="flex justify-between py-4 px-2 shadow-sm">
    <h1 class="text-sm font-bold">Where in the world?</h1>
  </header>

  <RouterView />
</template>

<style>
header {
  background: var(--secondary-background);
}
</style>

<template>
  <div class="min-h-screen bg-white">
    <Header v-if="!isProjectPage" />
    <main>
      <NuxtPage />
    </main>
  </div>
</template>

<script setup>
import { useRoute } from 'vue-router';
import { computed } from 'vue';

const route = useRoute();
const isProjectPage = computed(() => {
  return route.path.startsWith('/work/') && route.path !== '/work';
});
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600&display=swap');

body {
  font-family: 'Inter', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>
<template>
  <div class="min-h-screen bg-white flex flex-col">
    <Header v-if="!isProjectPage" />
    <main class="flex-grow">
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
  letter-spacing: -0.04em; /* -4% letter spacing */
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

/* Apply letter spacing to all text elements for consistency */
h1, h2, h3, h4, h5, h6, p, span, a, button, input, textarea, label {
  letter-spacing: -0.04em;
}
</style>
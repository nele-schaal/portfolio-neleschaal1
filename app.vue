<template>
  <Head>
    <link rel="icon" type="image/x-icon" href="/favicon_io/favicon.ico" />
    <link rel="icon" type="image/png" sizes="32x32" href="/favicon_io/favicon-32x32.png" />
    <link rel="icon" type="image/png" sizes="16x16" href="/favicon_io/favicon-16x16.png" />
    <link rel="apple-touch-icon" sizes="180x180" href="/favicon_io/apple-touch-icon.png" />
    <link rel="manifest" href="/favicon_io/site.webmanifest" />
    <link rel="icon" type="image/png" sizes="192x192" href="/favicon_io/android-chrome-192x192.png" />
    <link rel="icon" type="image/png" sizes="512x512" href="/favicon_io/android-chrome-512x512.png" />
  </Head>
  <div class="min-h-screen bg-white flex flex-col">
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
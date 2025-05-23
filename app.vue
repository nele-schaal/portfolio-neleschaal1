<template>
  <Head>
    <title>Nele Schaal</title>
    <meta name="description" content="Portfolio of Nele Schaal" />
    <link rel="icon" href="/favicon.ico" />
    <link rel="icon" type="image/png" href="/favicon/favicon-96x96-new.png" sizes="96x96" />
    <link rel="icon" type="image/svg+xml" href="/favicon/favicon-new.svg" />
    <link rel="shortcut icon" href="/favicon/favicon-new.ico" />
    <link rel="apple-touch-icon" sizes="180x180" href="/favicon/apple-touch-icon-new.png" />
    <link rel="manifest" href="/favicon/site.webmanifest" />
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
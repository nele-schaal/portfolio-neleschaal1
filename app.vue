<template>
  <div class="min-h-screen flex flex-col bg-white">
    <Header />
    <main class="flex-grow container mx-auto px-4 py-8">
      <NuxtPage />
    </main>
    <footer class="bg-gray-50 border-t">
      <div class="container mx-auto px-4 py-6 text-sm text-gray-600">
        <div class="flex justify-between items-center">
          <p>© 2024 Nele Schaal</p>
          <div class="space-x-4">
            <a href="mailto:your.email@example.com" class="hover:text-blue-700 transition">Email</a>
            <a href="https://linkedin.com" target="_blank" rel="noopener noreferrer" class="hover:text-blue-700 transition">LinkedIn</a>
          </div>
        </div>
      </div>
    </footer>
  </div>
</template>

<script setup lang="ts">
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600&display=swap');

body {
  font-family: 'Inter', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>
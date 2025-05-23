<template>
  <footer class="py-8 bg-white relative z-0 mt-8">
    <div class="max-w-[95%] mx-auto px-2">
      <div class="flex flex-col sm:flex-row justify-between items-center gap-2 sm:gap-4">
        <div class="text-base text-gray-500">
          © Nele Schaal 2025
        </div>
        <div class="text-base text-gray-500 hidden sm:block text-center flex-1">
          Vibecoded
        </div>
        <div class="flex flex-col sm:flex-row space-y-2 sm:space-y-0 sm:space-x-6 text-base text-gray-500 items-center">
          <a href="https://www.linkedin.com/in/nele-schaal/" target="_blank" rel="noopener noreferrer" class="hover:text-gray-900 transition-colors">
            LinkedIn
          </a>
          <span v-if="isImprint" class="text-gray-900 cursor-default">Imprint</span>
          <NuxtLink v-else to="/imprint" class="hover:text-gray-900 transition-colors">
            Imprint
          </NuxtLink>
        </div>
        <div class="text-base text-gray-500 sm:hidden text-center mt-2">
          Vibecoded
        </div>
      </div>
    </div>
  </footer>
</template>

<script setup lang="ts">
import { useRoute } from 'vue-router';
const props = defineProps({
  imprintLabel: { type: String, default: 'Imprint' }
});
const route = useRoute();
const isImprint = route.path === '/imprint';
</script>

<style scoped>
/* Any footer-specific styles */
</style> 
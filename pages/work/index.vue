<template>
  <div class="min-h-screen flex flex-col bg-white">
    <TheHeader />
    
    <main class="flex-grow pt-24 pb-16">
      <!-- Main content -->
      <div class="max-w-[95%] mx-auto px-2 py-4 sm:py-8">
        <h1 class="text-4xl sm:text-6xl font-bold mb-16 sm:mb-24">work</h1>
        
        <!-- Projects Grid -->
        <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
          <!-- Project Cards -->
          <NuxtLink 
            v-for="project in projects" 
            :key="project.title" 
            :to="`/work/${project.slug}`"
            class="block"
          >
            <div class="group relative aspect-[4/3] rounded-lg overflow-hidden bg-gray-100 mb-4">
              <img 
                v-if="project.image" 
                :src="project.image" 
                :alt="project.title"
                class="w-full h-full object-cover"
              />
              <div v-else class="w-full h-full flex items-center justify-center">
                <span class="text-2xl">{{ project.title }}</span>
              </div>
            </div>
            <h2 class="text-xl sm:text-2xl">{{ project.title }}</h2>
          </NuxtLink>
        </div>
      </div>
    </main>
    
    <TheFooter />
  </div>
</template>

<script setup lang="ts">
import TheFooter from '~/components/TheFooter.vue';
// ... existing script code ...
</script>

// ... existing style ... 
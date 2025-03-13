<template>
  <div class="min-h-screen bg-white">
    <div class="max-w-[95%] mx-auto px-2 py-8">
      <!-- Back button -->
      <NuxtLink to="/" class="inline-flex items-center mb-12 hover:opacity-70 transition-opacity">
        <img :src="chevronLeft" alt="Back" class="h-8 w-8">
      </NuxtLink>

      <!-- Project Header -->
      <div class="mb-12">
        <h1 class="text-3xl font-bold mb-2">{{ project?.title }}</h1>
        <p v-if="project?.subtitle" class="text-lg">{{ project?.subtitle }}</p>
      </div>

      <!-- Project Metadata -->
      <div class="grid grid-cols-2 gap-8 mb-12 max-w-lg">
        <div>
          <p class="text-sm text-gray-500 mb-2">(Team)</p>
          <div v-if="project?.team" class="space-y-1">
            <p v-for="member in project?.team" :key="member" class="text-base">{{ member }}</p>
          </div>
        </div>
        <div>
          <p class="text-sm text-gray-500 mb-2">(Time)</p>
          <p v-if="project?.time" class="text-base">{{ project?.time }}</p>
        </div>
      </div>

      <!-- Project Content -->
      <div class="w-full">
        <div v-if="project?.image" class="w-full bg-gray-100 rounded-lg overflow-hidden mb-12">
          <img :src="project?.image" :alt="project?.title" class="w-full h-auto object-contain" />
        </div>
        
        <!-- Add more project details here -->
        <div v-if="project?.description" class="mb-12">
          <p class="text-lg">{{ project?.description }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
// No longer need to define layout
// definePageMeta({
//   layout: 'project'
// });

import { ref, onMounted } from 'vue';
import { useRoute } from 'vue-router';
import pulseImage from '~/assets/pulse.png';
import alexImage from '~/assets/alex.png';
import chevronLeft from '~/assets/chevron-left.svg';

const route = useRoute();
const projectSlug = route.params.project as string;

interface Project {
  title: string;
  subtitle?: string;
  image?: string;
  description?: string;
  team?: string[];
  time?: string;
}

const projectsData: Record<string, Project> = {
  alex: {
    title: 'alex',
    image: alexImage,
    description: 'Description for alex project'
  },
  pulse: {
    title: 'Pulse',
    subtitle: 'Designing a blood donation app',
    image: pulseImage,
    team: ['Leon Bork', 'Enes Cilingir', 'Nele Schaal'],
    time: '5 months',
    description: 'Detailed description of the Pulse project.'
  },
  sense: {
    title: 'Sense',
    description: 'Description for Sense project'
  }
};

const project = ref<Project | null>(projectsData[projectSlug as keyof typeof projectsData] || null);
</script>

<style scoped>
/* Removing fixed aspect ratio constraint to prevent cropping */
</style> 
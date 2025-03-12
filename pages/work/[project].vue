<template>
  <div class="min-h-screen bg-white">
    <div class="container mx-auto px-4 py-24">
      <!-- Back button -->
      <NuxtLink to="/" class="inline-flex items-center text-sm mb-12 hover:opacity-70 transition-opacity">
        <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 mr-1" fill="none" viewBox="0 0 24 24" stroke="currentColor">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 19l-7-7 7-7" />
        </svg>
        back
      </NuxtLink>

      <!-- Project Content -->
      <div v-if="project" class="max-w-4xl mx-auto">
        <h1 class="text-4xl font-bold mb-8">{{ project.title }}</h1>
        <div v-if="project.image" class="aspect-[16/9] rounded-lg overflow-hidden mb-12">
          <img :src="project.image" :alt="project.title" class="w-full h-full object-cover" />
        </div>
        <!-- Add more project details here -->
      </div>
      <div v-else class="text-center">
        <p>Project not found</p>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue';
import { useRoute } from 'vue-router';
import pulseImage from '~/assets/pulse.png';
import alexImage from '~/assets/alex.png';

const route = useRoute();
const projectSlug = route.params.project as string;

interface Project {
  title: string;
  image?: string;
  description?: string;
}

const projectsData = {
  alex: {
    title: 'alex',
    image: alexImage,
    description: 'Description for alex project'
  },
  pulse: {
    title: 'Pulse',
    image: pulseImage,
    description: 'Description for Pulse project'
  },
  sense: {
    title: 'Sense',
    description: 'Description for Sense project'
  }
};

const project = ref<Project | null>(projectsData[projectSlug as keyof typeof projectsData] || null);
</script> 
<template>
  <div class="space-y-16">
    <!-- Hero Section -->
    <section class="py-16">
      <div class="max-w-3xl">
        <h1 class="text-4xl font-semibold mb-6">Interaction Design Student at HfG Schwäbisch Gmünd</h1>
        <p class="text-xl text-gray-600 leading-relaxed">
          Creating meaningful digital experiences through user-centered design and creative problem-solving.
        </p>
      </div>
    </section>

    <!-- Projects Grid -->
    <section>
      <h2 class="text-2xl font-semibold mb-8">Selected Works</h2>
      <div class="grid grid-cols-1 md:grid-cols-2 gap-8">
        <div v-for="project in projects" :key="project.title" class="group">
          <div class="aspect-[4/3] bg-gray-100 mb-4 overflow-hidden flex items-center justify-center">
            <!-- Placeholder until you add project images -->
            <div class="text-gray-400">{{ project.title }} Preview</div>
          </div>
          <h3 class="text-lg font-medium mb-2">{{ project.title }}</h3>
          <p class="text-gray-600">{{ project.description }}</p>
        </div>
      </div>
    </section>
  </div>
</template>

<script setup lang="ts">
const projects = [
  {
    title: "Digital Experience Design",
    description: "User interface and interaction design for a digital platform",
    image: "/images/project-1.jpg"
  },
  {
    title: "Interactive Installation",
    description: "Physical computing project exploring human-computer interaction",
    image: "/images/project-2.jpg"
  },
  {
    title: "Mobile App Design",
    description: "UX/UI design for a sustainable living application",
    image: "/images/project-3.jpg"
  },
  {
    title: "Design Research",
    description: "User research and concept development for future interfaces",
    image: "/images/project-4.jpg"
  }
];
</script>

<style scoped>
</style>
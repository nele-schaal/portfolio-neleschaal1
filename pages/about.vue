<template>
  <div class="max-w-4xl">
    <!-- Bio Section -->
    <section class="mb-16">
      <h1 class="text-4xl font-semibold mb-8">About Me</h1>
      <div class="grid md:grid-cols-[2fr,1fr] gap-12">
        <div class="space-y-6">
          <p class="text-lg text-gray-600 leading-relaxed">
            I'm an interaction design student at HfG Schwäbisch Gmünd, passionate about creating meaningful digital experiences that bridge the gap between humans and technology.
          </p>
          <p class="text-lg text-gray-600 leading-relaxed">
            My approach combines user-centered design principles with creative problem-solving, always striving to create interfaces and interactions that are both intuitive and engaging.
          </p>
          <p class="text-lg text-gray-600 leading-relaxed">
            Currently focusing on [your specific interests/focus areas], I'm always excited to collaborate on projects that push the boundaries of digital interaction.
          </p>
        </div>
        <div class="bg-gray-100 aspect-[3/4] flex items-center justify-center">
          <!-- Placeholder until you add your profile image -->
          <div class="text-gray-400">Profile Image Placeholder</div>
        </div>
      </div>
    </section>

    <!-- Skills Section -->
    <section class="mb-16">
      <h2 class="text-2xl font-semibold mb-6">Skills & Expertise</h2>
      <div class="grid grid-cols-2 md:grid-cols-3 gap-6">
        <div v-for="category in skills" :key="category.title" class="space-y-3">
          <h3 class="font-medium text-lg">{{ category.title }}</h3>
          <ul class="space-y-2 text-gray-600">
            <li v-for="skill in category.items" :key="skill">{{ skill }}</li>
          </ul>
        </div>
      </div>
    </section>

    <!-- Education Section -->
    <section>
      <h2 class="text-2xl font-semibold mb-6">Education</h2>
      <div class="space-y-4">
        <div>
          <h3 class="font-medium">HfG Schwäbisch Gmünd</h3>
          <p class="text-gray-600">Bachelor in Interaction Design</p>
          <p class="text-gray-500">Expected graduation: [Year]</p>
        </div>
      </div>
    </section>
  </div>
</template>

<script setup lang="ts">
const skills = [
  {
    title: "Design",
    items: [
      "User Interface Design",
      "Interaction Design",
      "Prototyping",
      "User Experience",
      "Design Systems"
    ]
  },
  {
    title: "Tools",
    items: [
      "Figma",
      "Adobe Creative Suite",
      "Sketch",
      "Principle",
      "Framer"
    ]
  },
  {
    title: "Development",
    items: [
      "HTML/CSS",
      "JavaScript",
      "Vue.js",
      "Processing",
      "Arduino"
    ]
  }
];
</script>

<style scoped>
</style>
<template>
  <div class="min-h-screen flex flex-col bg-white">
    <TheHeader />
    
    <main class="flex-grow pb-16 pt-20">
      <div class="max-w-[95%] mx-auto px-2">
        <div class="container mx-auto px-2 mt-12">
          <div class="grid grid-cols-1 md:grid-cols-2 gap-8 md:gap-16 items-center">
            <!-- Profile Image -->
            <div class="aspect-square rounded-lg overflow-hidden max-w-xs mx-auto md:max-w-full">
              <img 
                src="~/assets/ich.png" 
                alt="Profile picture" 
                class="w-full h-full object-contain"
              />
            </div>
            <!-- Bio Content -->
            <div class="flex flex-col justify-center h-full space-y-4 text-center md:text-left mt-8 md:mt-0 max-w-prose w-full mx-auto md:mx-0">
              <h1 class="text-3xl sm:text-4xl font-semibold mb-2">Hi, I am Nele!</h1><br />
              <p class="text-base sm:text-lg">
                I'm an Interaction Design student at the University of Applied<br />Sciences in Schwäbisch Gmünd and currently looking for an <br /> internship.<br /><br />
                Driven by curiosity and a deep interest in how humans and <br />technology connect, I enjoy bringing new ideas to life through <br /> thoughtful concepts and hands-on prototypes.<br>
                I find inspiration in nature, conversations, books, and everyday <br />interactions. I love diving deep into topics and exploring how we <br />can shape the future through the connection of technology and <br />design.
              </p>
            </div>
          </div>
        </div>
      </div>
    </main>
    
    <TheFooter />
  </div>
</template>

<script setup lang="ts">
import TheFooter from '~/components/TheFooter.vue';
// Any about page specific logic here
</script>

<style scoped>
</style>
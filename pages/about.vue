<template>
  <div class="min-h-screen flex flex-col bg-white">
    <TheHeader />
    
    <main class="flex-grow pb-16 pt-22">
      <div class="max-w-[95%] mx-auto px-2">
        <div class="container mx-auto px-2 mt-4 sm:mt-12">
          <div class="grid grid-cols-1 md:grid-cols-2 gap-8 md:gap-16 items-center">
            <!-- Profile Image -->
            <div class="aspect-square rounded-lg overflow-hidden max-w-xs mx-auto md:max-w-full">
              <img 
                src="~/assets/ich.jpg" 
                alt="Profile picture" 
                class="w-full h-full object-contain"
              />
            </div>
            <!-- Bio Content -->
            <div class="flex flex-col justify-center h-full space-y-4 text-left mt-8 md:mt-0 max-w-prose w-full mx-auto md:mx-0">
              <h1 class="text-3xl sm:text-4xl font-semibold mb-2">Hi, I am Nele!</h1><br />
              <!-- Desktop text (shown on sm and up) -->
              <p class="text-base sm:text-lg hidden sm:block">
                I'm an Interaction Design student at the University of Applied<br />Sciences in Schwäbisch Gmünd and currently looking for an <br /> internship.<br /><br />
                Driven by curiosity and a deep interest in how humans and <br />technology connect, I enjoy bringing new ideas to life through <br /> thoughtful concepts and hands-on prototypes.<br>
                I find inspiration in nature, conversations, books, and everyday <br />interactions, and I love diving deep into topics and exploring how <br /> we can shape the future through the connection of technology <br /> and design, creating meaningful, user-centered solutions that <br />respond to real needs.
              </p>
              <!-- Mobile text (shown only on mobile) -->
              <p class="text-base sm:text-lg block sm:hidden">
                I'm an Interaction Design student at <br />the University of Applied Sciences<br />in Schwäbisch Gmünd and currently<br /> looking for an internship.<br /><br />
                Driven by curiosity and a deep interest<br /> in how humans and technology connect,<br />I enjoy bringing new ideas to life through thoughtful concepts and hands-on prototypes.<br><br />
                I find inspiration in nature, conversations, books, and everyday interactions,<br />and I love diving deep into topics and exploring how we can shape the future through the connection of technology and design, creating meaningful, user-centered solutions that respond to real needs.
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
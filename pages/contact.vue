<template>
  <div class="min-h-screen flex flex-col bg-white">
    <TheHeader />
    
    <main class="flex-grow pt-24 pb-16">
      <div class="max-w-[95%] mx-auto px-2">
        <h1 class="text-xl font-normal mb-10">contact me</h1>
        
        <div class="max-w-2xl">
          <div class="space-y-10">
            <div>
              <p class="text-lg mb-4">
                I'm always open for interesting projects and collaborations.
                Feel free to reach out to discuss your ideas.
              </p>
            </div>
            
            <div>
              <h2 class="text-lg font-medium mb-4">Connect</h2>
              <ul class="space-y-2">
                <li>
                  <a href="https://www.linkedin.com/in/neleschaal" target="_blank" rel="noopener noreferrer" class="text-base hover:opacity-70 transition-opacity">
                    LinkedIn: /neleschaal
                  </a>
                </li>
                <li>
                  <p class="text-base">Email: nele.schaal@hfg-gmuend.de</p>
                </li>
              </ul>
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
// Any contact page specific logic here
</script>

<style scoped>
/* Contact page specific styles */
</style> 
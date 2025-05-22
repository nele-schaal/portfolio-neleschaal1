<!-- <template> 
  <div>
    <NuxtLink to="/">Home</NuxtLink>
    <NuxtLink to="/about">About</NuxtLink>
  </div>
</template>

<script lang="ts" setup>

</script>

<style>

</style> -->

<template>
  <header class="py-4 sm:py-8">
    <nav class="container mx-auto px-4 flex justify-between items-center text-base sm:text-lg">
      <div class="space-x-1 sm:space-x-2">
        <NuxtLink to="/" class="hover:opacity-70 transition">work</NuxtLink>
        <span class="text-gray-400">//</span>
        <NuxtLink to="/about" class="hover:opacity-70 transition">about</NuxtLink>
      </div>
      <NuxtLink to="/" class="text-center font-bold">nele schaal</NuxtLink>
      <NuxtLink to="/contact" class="hover:opacity-70 transition hidden sm:inline-block">contact me</NuxtLink>
      <NuxtLink to="/contact" class="hover:opacity-70 transition sm:hidden">contact</NuxtLink>
    </nav>
  </header>
</template>

<script setup lang="ts">
</script>

<style scoped>
</style>

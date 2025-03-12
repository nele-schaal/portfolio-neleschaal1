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
  <header class="py-8">
    <nav class="container mx-auto px-4 flex justify-between items-center">
      <div class="space-x-2">
        <NuxtLink to="/" class="hover:opacity-70 transition">work</NuxtLink>
        <span class="text-gray-400">//</span>
        <NuxtLink to="/about" class="hover:opacity-70 transition">about</NuxtLink>
      </div>
      <NuxtLink to="/" class="text-center font-bold">nele schaal</NuxtLink>
      <NuxtLink to="/contact" class="hover:opacity-70 transition">contact me</NuxtLink>
    </nav>
  </header>
</template>

<script setup lang="ts">
</script>

<style scoped>
</style>

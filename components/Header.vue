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
  <header class="bg-white text-black shadow-md">
    <nav class="container mx-auto flex justify-between items-center p-4">
      <NuxtLink to="/" class="text-xl font-medium text-blue-700">nele schaal</NuxtLink>
      <div class="space-x-6">
        <NuxtLink to="/" class="hover:text-blue-700 transition">work</NuxtLink>
        <NuxtLink to="/about" class="hover:text-blue-700 transition">about</NuxtLink>
      </div>
    </nav>
  </header>
</template>

<script lang="ts" setup>

</script>

<style scoped>
</style>

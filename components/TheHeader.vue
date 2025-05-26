<template>
  <header class="fixed top-0 left-0 right-0 bg-white z-10 h-[92px]">
    <div class="max-w-[95%] mx-auto px-2 py-4 sm:py-6 h-full">
      <!-- Desktop Navigation -->
      <div class="hidden md:flex items-center h-full w-full">
        <!-- Left: work/about -->
        <div class="flex items-center flex-1 min-w-0">
          <NuxtLink 
            to="/" 
            @click.prevent="navigateToWork"
            class="text-base sm:text-lg text-black transition-opacity relative nav-link"
            :class="{ 'active': isWorkActive }"
          >work</NuxtLink>
          <span class="mx-8 font-light text-base sm:text-lg"></span>
          <NuxtLink 
            to="/about" 
            class="text-base sm:text-lg text-black transition-opacity relative nav-link font-light"
            :class="{ 'active': route.path === '/about' }"
          >about</NuxtLink>
        </div>
        <!-- Center: nele schaal -->
        <div class="flex-1 flex justify-center min-w-0">
          <NuxtLink to="/" class="text-lg sm:text-xl text-black transition-opacity relative font-semibold whitespace-nowrap">nele schaal</NuxtLink>
        </div>
        <!-- Right: contact me -->
        <div class="flex-1 flex justify-end min-w-0">
          <a 
            href="mailto:nele.schaal@hfg-gmuend.de" 
          class="text-base sm:text-lg text-black transition-opacity relative nav-link font-normal"
          >contact me</a>
        </div>
      </div>
      <!-- Mobile Hamburger -->
      <div class="md:hidden flex items-center justify-between w-full">
        <NuxtLink to="/" class="text-lg font-semibold">nele schaal</NuxtLink>
        <button @click="showMobileNav = !showMobileNav" class="focus:outline-none p-2">
          <svg class="w-7 h-7" fill="none" stroke="currentColor" stroke-width="2" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" d="M4 6h16M4 12h16M4 18h16" />
          </svg>
        </button>
      </div>
      <transition name="fade">
        <div v-if="showMobileNav" class="fixed inset-0 bg-white z-50 flex flex-col items-center justify-center space-y-8 text-2xl">
          <button @click="navigateToWorkMobile" class="focus:outline-none">work</button>
          <NuxtLink to="/about" @click.native="showMobileNav = false">about</NuxtLink>
          <a href="mailto:nele.schaal@hfg-gmuend.de" @click.native="showMobileNav = false">contact me</a>
          <button @click="showMobileNav = false" class="absolute top-6 right-6 text-3xl">&times;</button>
        </div>
      </transition>
    </div>
  </header>
</template>

<script setup lang="ts">
import { computed, ref, onMounted, onUnmounted } from 'vue';

const route = useRoute();
const router = useRouter();
const isScrolledToWork = ref(false);
const showMobileNav = ref(false);

// Computed property to determine if work is active
const isWorkActive = computed(() => {
  // Only active on work routes, not home route
  if (route.path === '/') {
    return isScrolledToWork.value;
  }
  return route.path.startsWith('/work');
});

// Check if user has scrolled to work section
function checkWorkSectionVisibility() {
  const workSection = document.getElementById('work');
  if (workSection && route.path === '/') {
    const rect = workSection.getBoundingClientRect();
    // Consider it visible when the top of the section is within the viewport
    isScrolledToWork.value = rect.top <= 100;
  }
}

// Set up scroll listener
onMounted(() => {
  if (route.path === '/') {
    window.addEventListener('scroll', checkWorkSectionVisibility);
    // Initial check
    checkWorkSectionVisibility();
  }
});

onUnmounted(() => {
  window.removeEventListener('scroll', checkWorkSectionVisibility);
});

async function navigateToWork(event: Event) {
  event.preventDefault();
  if (route.path !== '/') {
    await router.push('/');
    // Wait for the next DOM update cycle
    await nextTick();
    // Add a small delay to ensure the section is mounted
    await new Promise(resolve => setTimeout(resolve, 100));
  }
  const workSection = document.getElementById('work');
  if (workSection) {
    workSection.scrollIntoView({ behavior: 'smooth' });
  }
}

// Mobile: Scroll to work section or navigate home and scroll
async function navigateToWorkMobile() {
  showMobileNav.value = false;
  if (route.path !== '/') {
    await router.push('/');
    await nextTick();
    await new Promise(resolve => setTimeout(resolve, 100));
  }
  const workSection = document.getElementById('work');
  if (workSection) {
    workSection.scrollIntoView({ behavior: 'smooth' });
  }
}
</script>

<style scoped>
.nav-link {
  position: relative;
}

.nav-link::after {
  content: '';
  position: absolute;
  width: 0;
  height: 1.8px;
  bottom: 1px;
  left: 0;
  background-color: black;
  transition: width 0.2s ease-in-out;
}

.nav-link:hover::after,
.nav-link.active::after {
  width: 100%;
}
</style> 
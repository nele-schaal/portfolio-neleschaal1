<template>
  <div class="min-h-screen flex flex-col relative bg-white">
    <TheHeader />

    <!-- Drawing Canvas (hidden on mobile) -->
    <canvas
      ref="canvas"
      class="absolute inset-0 w-full h-full hidden sm:block"
      style="touch-action: none;"
    ></canvas>

    <!-- Custom Cursor (hidden on mobile) -->
    <div v-show="shouldShowCursor && !isHoveringHeaderLink && !isHoveringHeader" ref="customCursor" class="custom-cursor hidden sm:flex">
      <div class="flex items-center">
        <img :src="pencilIcon" alt="Pencil" class="w-4 h-4">
        <span class="text-sm ml-1">draw</span>
      </div>
    </div>

    <!-- Text Section -->
    <div class="flex flex-col items-center justify-center text-center px-4 pointer-events-none mt-10" style="min-height: calc(100vh - 92px);">
      <div class="max-w-4xl">
        <h1 class="text-4xl sm:text-6xl md:text-7xl lg:text-8xl font-bold mb-2">
          I design, question,<br>
          prototype, and play.
        </h1>
      </div>
    </div>

    <!-- Scroll Down Arrow -->
    <div class="absolute bottom-10 left-1/2 transform -translate-x-1/2 cursor-pointer" @click="scrollToWork">
      <img :src="arrowIcon" alt="Scroll Down" class="w-8 h-8 sm:w-10 sm:h-10 animate-bounce">
    </div>
  </div>

  <!-- Work Section -->
  <section id="work" class="pt-8 pb-16 bg-white opacity-0 translate-y-10 transition-all duration-700 ease-out scroll-mt-24" ref="workSection">
    <div class="max-w-[95%] mx-auto px-2 mb-2">
      <div class="grid grid-cols-1 sm:grid-cols-2 gap-6 mt-0">
        <NuxtLink 
          v-for="project in projects" 
          :key="project.title" 
          :to="`/work/${project.title.toLowerCase()}`" 
          class="relative group cursor-pointer"
        >
          <div v-if="project.image" class="aspect-[4/3] rounded-lg overflow-hidden transition-transform duration-300 group-hover:scale-[1.02]">
            <img :src="project.image" :alt="project.title" class="w-full h-full object-cover transition-transform duration-300 group-hover:scale-105" />
          </div>
          <div v-else class="aspect-[4/3] bg-gray-200 rounded-lg transition-transform duration-300 group-hover:scale-[1.02]"></div>
          <p class="absolute bottom-4 left-4 text-black transition-opacity duration-300 opacity-0 group-hover:opacity-100 text-xl sm:text-2xl">{{ project.title }}</p>
        </NuxtLink>
      </div>
    </div>
  </section>
  
  <!-- Footer -->
  <TheFooter />
</template>

<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue';
import TheFooter from '~/components/TheFooter.vue';

// Import the images directly for better asset handling
import pulseImage from '~/assets/pulse/pulse.png';
import alexImage from '~/assets/alex/alex.png';
import senseImage from '~/assets/sense/sense.png';
import soundPlotterImage from '~/assets/soundplotter/soundplotter.png';
// Import your custom SVG icons (replace with your actual filenames)
import pencilIcon from '~/assets/pencil-icon.svg'; // Your custom pencil icon as SVG
import arrowIcon from '~/assets/arrow-icon.svg';   // Your custom arrow icon as SVG

const canvas = ref<HTMLCanvasElement | null>(null);
const customCursor = ref<HTMLElement | null>(null);
let ctx: CanvasRenderingContext2D | null = null;
let isDrawing = ref(false);
let lastX = 0;
let lastY = 0;
let clearTimer: ReturnType<typeof setTimeout> | undefined;

// Store drawn lines
interface Point {
  x: number;
  y: number;
}

interface Line {
  start: Point;
  end: Point;
}

const currentPath = ref<Line[]>([]);
const shouldClearPath = ref(false);

// Add shouldShowCursor to track visibility
const shouldShowCursor = ref(true);
// Track when hovering over header links
const isHoveringHeaderLink = ref(false);
// Add isHoveringHeader ref
const isHoveringHeader = ref(false);

const projects = [
  { title: "alex", image: alexImage },
  { title: "Sense", image: senseImage },
  { title: "Sound Plotter", image: soundPlotterImage },
  { title: "Pulse", image: pulseImage }
];

const workSection = ref<HTMLElement | null>(null);

// Simple drawing with explicit management of the clear timer
function startDrawing(e: MouseEvent) {
  if (!shouldShowCursor.value || isHoveringHeaderLink.value || !canvas.value || !ctx) return;
  
  // Cancel any pending clear operation
  if (clearTimer) {
    clearTimeout(clearTimer);
    clearTimer = undefined;
  }
  
  isDrawing.value = true;
  const rect = canvas.value.getBoundingClientRect();
  lastX = e.clientX - rect.left;
  lastY = e.clientY - rect.top;
}

function draw(e: MouseEvent) {
  if (!isDrawing.value || !ctx || !canvas.value) return;
  
  const rect = canvas.value.getBoundingClientRect();
  const x = e.clientX - rect.left;
  const y = e.clientY - rect.top;
  
  ctx.beginPath();
  ctx.moveTo(lastX, lastY);
  ctx.lineTo(x, y);
  ctx.stroke();
  
  lastX = x;
  lastY = y;
}

function stopDrawing() {
  if (!isDrawing.value) return;
  
  isDrawing.value = false;
  
  // Schedule clearing of the canvas in 1.5 seconds
  if (clearTimer) {
    clearTimeout(clearTimer);
  }
  
  console.log('Scheduling canvas clearing in 2 seconds');
  clearTimer = setTimeout(() => {
    console.log('Clearing canvas now');
    if (ctx && canvas.value) {
      ctx.clearRect(0, 0, canvas.value.width, canvas.value.height);
    }
    clearTimer = undefined;
  }, 1500);
}

function resizeCanvas() {
  if (!canvas.value || !ctx) return;
  
  const rect = canvas.value.getBoundingClientRect();
  
  // Account for device pixel ratio for high resolution
  const dpr = window.devicePixelRatio || 1;
  
  // Set the canvas dimensions scaled by device pixel ratio
  canvas.value.width = rect.width * dpr;
  canvas.value.height = rect.height * dpr;
  
  // Scale the context to counter the pixel ratio scaling
  ctx.scale(dpr, dpr);
  
  // Set the CSS size
  canvas.value.style.width = `${rect.width}px`;
  canvas.value.style.height = `${rect.height}px`;
  
  // Set better line quality settings
  ctx.lineCap = 'round';
  ctx.lineJoin = 'round';
  ctx.strokeStyle = '#00008b';
  ctx.lineWidth = 4;
}

function updateCursor(e: MouseEvent) {
  if (!shouldShowCursor.value) return;
  
  // Check if mouse is in header area (top 92px of the page)
  if (e.clientY <= 92) {
    isHoveringHeader.value = true;
    isHoveringHeaderLink.value = true;
  } else {
    isHoveringHeader.value = false;
    isHoveringHeaderLink.value = false;
  }
  
  document.documentElement.style.setProperty('--cursor-x', `${e.clientX}px`);
  document.documentElement.style.setProperty('--cursor-y', `${e.clientY}px`);
}

function handleScroll() {
  const scrollPosition = window.scrollY;
  const windowHeight = window.innerHeight;
  // Make the cursor disappear earlier, right when reaching the arrow icon
  // Lowering threshold significantly to ensure it disappears at the arrow
  shouldShowCursor.value = scrollPosition < windowHeight * 0.35;
  
  if (!shouldShowCursor.value && isDrawing.value) {
    stopDrawing();
  }
}

// Update header hover handlers
function setupHeaderHoverListeners() {
  // Remove the old event listeners since we're now handling this in updateCursor
  return;
}

function scrollToWork() {
  const workSection = document.getElementById('work');
  if (workSection) {
    workSection.scrollIntoView({ behavior: 'smooth' });
  }
}

onMounted(() => {
  if (!canvas.value) return;
  
  ctx = canvas.value.getContext('2d');
  resizeCanvas();

  // Use direct mouse events without intermediaries
  canvas.value.addEventListener('mousedown', startDrawing);
  canvas.value.addEventListener('mousemove', draw);
  canvas.value.addEventListener('mouseup', stopDrawing);
  canvas.value.addEventListener('mouseleave', stopDrawing);
  
  window.addEventListener('resize', resizeCanvas);
  window.addEventListener('mousemove', updateCursor);
  window.addEventListener('scroll', handleScroll);
  
  // Setup header hover listeners after a short delay to ensure DOM is ready
  setTimeout(setupHeaderHoverListeners, 500);

  // Set up Intersection Observer for work section animation
  const observer = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
      if (entry.isIntersecting) {
        const section = entry.target as HTMLElement;
        section.style.opacity = '1';
        section.style.transform = 'translateY(0)';
        observer.unobserve(section); // Stop observing once animation is triggered
      }
    });
  }, {
    threshold: 0.1 // Trigger when 10% of the section is visible
  });

  if (workSection.value) {
    observer.observe(workSection.value);
  }
});

onUnmounted(() => {
  if (canvas.value) {
    canvas.value.removeEventListener('mousedown', startDrawing);
    canvas.value.removeEventListener('mousemove', draw);
    canvas.value.removeEventListener('mouseup', stopDrawing);
    canvas.value.removeEventListener('mouseleave', stopDrawing);
  }
  
  window.removeEventListener('resize', resizeCanvas);
  window.removeEventListener('mousemove', updateCursor);
  window.removeEventListener('scroll', handleScroll);
  
  if (clearTimer) {
    clearTimeout(clearTimer);
  }
  
  // Clean up header hover listeners
  const header = document.querySelector('header');
  if (header) {
    header.removeEventListener('mouseenter', () => {
      isHoveringHeader.value = true;
      isHoveringHeaderLink.value = true;
    });
    header.removeEventListener('mouseleave', () => {
      isHoveringHeader.value = false;
      isHoveringHeaderLink.value = false;
    });
  }
});
</script>

<style scoped>
canvas {
  touch-action: none;
}

:root {
  --cursor-x: 0px;
  --cursor-y: 0px;
}

.custom-cursor {
  position: fixed;
  top: 0;
  left: 0;
  z-index: 9999;
  pointer-events: none;
  transform: translate(calc(var(--cursor-x) + 15px), calc(var(--cursor-y)));
  display: flex;
  align-items: center;
}

.nav-link {
  position: relative;
}

.nav-link::after {
  content: '';
  position: absolute;
  width: 0;
  height: 1.5px;
  bottom: 2px;
  left: 0;
  background-color: black;
  transition: width 0.2s ease-in-out;
}

.nav-link:hover::after {
  width: 100%;
}
</style>
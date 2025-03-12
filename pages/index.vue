<template>
  <div class="min-h-screen flex flex-col relative bg-white">
    <!-- Drawing Canvas -->
    <canvas
      ref="canvas"
      class="absolute inset-0 w-full h-full"
      style="touch-action: none;"
    ></canvas>

    <!-- Custom Cursor -->
    <div v-show="shouldShowCursor" ref="customCursor" class="custom-cursor">
      <div class="flex items-center">
        <div class="w-2 h-2 bg-black rounded-full mr-1"></div>
        <svg xmlns="http://www.w3.org/2000/svg" class="w-4 h-4" fill="none" viewBox="0 0 24 24" stroke="currentColor">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 20h9" />
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M16.5 3.5l4 4L7 21H3v-4L16.5 3.5z" />
        </svg>
        <span class="text-sm ml-1">draw</span>
      </div>
    </div>

    <!-- Text Section -->
    <div class="flex-grow flex flex-col items-center justify-center text-center px-4 pointer-events-none">
      <p class="text-7xl font-bold mb-2">I design, question, prototype, and play.</p>
      <p class="text-2xl font-bold">Always searching for better interactions.</p>
    </div>

    <!-- Scroll Down Arrow -->
    <div class="absolute bottom-12 left-1/2 transform -translate-x-1/2">
      <svg xmlns="http://www.w3.org/2000/svg" class="w-6 h-6 animate-bounce" fill="none" viewBox="0 0 24 24" stroke="currentColor">
        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 9l-7 7-7-7" />
      </svg>
    </div>
  </div>

  <!-- Work Section -->
  <section class="pt-24 pb-16 bg-white border-t border-gray-200">
    <div class="container mx-auto px-4">
      <h2 class="text-xl font-normal mb-10">work</h2>
      <div class="grid grid-cols-1 md:grid-cols-2 gap-8">
        <div v-for="project in projects" :key="project.title" class="relative">
          <div class="aspect-[4/3] bg-gray-200 rounded-lg"></div>
          <p class="absolute bottom-4 left-4 text-black">{{ project.title }}</p>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue';

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

const projects = [
  { title: "alex" },
  { title: "Pulse" },
  { title: "Sense" },
  { title: "Project 4" }
];

// Simple drawing with explicit management of the clear timer
function startDrawing(e: MouseEvent) {
  if (!shouldShowCursor.value || !canvas.value || !ctx) return;
  
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
  
  // Schedule clearing of the canvas in 3 seconds
  if (clearTimer) {
    clearTimeout(clearTimer);
  }
  
  console.log('Scheduling canvas clearing in 3 seconds');
  clearTimer = setTimeout(() => {
    console.log('Clearing canvas now');
    if (ctx && canvas.value) {
      ctx.clearRect(0, 0, canvas.value.width, canvas.value.height);
    }
    clearTimer = undefined;
  }, 3000);
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
  ctx.lineWidth = 2;
}

function updateCursor(e: MouseEvent) {
  if (shouldShowCursor.value) {
    document.documentElement.style.setProperty('--cursor-x', `${e.clientX}px`);
    document.documentElement.style.setProperty('--cursor-y', `${e.clientY}px`);
  }
}

function handleScroll() {
  const scrollPosition = window.scrollY;
  const windowHeight = window.innerHeight;
  
  shouldShowCursor.value = scrollPosition < windowHeight * 0.8;
  
  if (!shouldShowCursor.value && isDrawing.value) {
    stopDrawing();
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
</style>
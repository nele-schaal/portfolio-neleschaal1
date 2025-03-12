<template>
  <div class="min-h-screen flex flex-col relative bg-white">
    <!-- Drawing Canvas -->
    <canvas
      ref="canvas"
      class="absolute inset-0 w-full h-full"
      style="touch-action: none;"
    ></canvas>

    <!-- Custom Cursor -->
    <div ref="customCursor" class="fixed flex items-center space-x-1 pointer-events-none">
      <div class="w-2 h-2 bg-black rounded-full"></div>
      <svg xmlns="http://www.w3.org/2000/svg" class="w-4 h-4" fill="none" viewBox="0 0 24 24" stroke="currentColor">
        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 20h9" />
        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M16.5 3.5l4 4L7 21H3v-4L16.5 3.5z" />
      </svg>
      <span class="text-sm">draw</span>
    </div>

    <!-- Text Section -->
    <div class="flex-grow flex flex-col items-center justify-center text-center px-4 pointer-events-none">
      <p class="text-7xl font-bold mb-2">I design, question, prototype, and play.</p>
      <p class="text-2xl font-bold">Always searching for better interactions.</p>
    </div>

    <!-- Scroll Down Arrow -->
    <div class="absolute bottom-30 left-1/2 transform -translate-x-1/2">
      <svg xmlns="http://www.w3.org/2000/svg" class="w-6 h-6 animate-bounce" fill="none" viewBox="0 0 24 24" stroke="currentColor">
        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 9l-7 7-7-7" />
      </svg>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue';

const canvas = ref<HTMLCanvasElement | null>(null);
const customCursor = ref<HTMLElement | null>(null);
let ctx: CanvasRenderingContext2D | null = null;
let isDrawing = false;
let lastX = 0;
let lastY = 0;
let clearTimeoutId: number | null = null;

function startDrawing(e: MouseEvent) {
  if (clearTimeoutId !== null) {
    clearTimeout(clearTimeoutId);
  }
  isDrawing = true;
  const rect = canvas.value?.getBoundingClientRect();
  if (rect) {
    lastX = e.clientX - rect.left;
    lastY = e.clientY - rect.top;
  }
}

function draw(e: MouseEvent) {
  if (!isDrawing || !ctx || !canvas.value) return;
  
  const rect = canvas.value.getBoundingClientRect();
  const x = e.clientX - rect.left;
  const y = e.clientY - rect.top;

  ctx.beginPath();
  ctx.moveTo(lastX, lastY);
  ctx.lineTo(x, y);
  ctx.strokeStyle = '#00008b';
  ctx.lineWidth = 2;
  ctx.lineCap = 'round';
  ctx.stroke();
  
  lastX = x;
  lastY = y;
}

function stopDrawing() {
  isDrawing = false;
  if (clearTimeoutId !== null) {
    clearTimeout(clearTimeoutId);
  }
  clearTimeoutId = window.setTimeout(clearCanvas, 4000);
}

function resizeCanvas() {
  if (!canvas.value || !ctx) return;
  
  const rect = canvas.value.getBoundingClientRect();
  canvas.value.width = rect.width;
  canvas.value.height = rect.height;
}

function clearCanvas() {
  if (ctx && canvas.value) {
    ctx.clearRect(0, 0, canvas.value.width, canvas.value.height);
  }
}

function toggleDrawing(e: MouseEvent) {
  if (isDrawing) {
    stopDrawing();
  } else {
    if (clearTimeoutId !== null) {
      clearTimeout(clearTimeoutId);
    }
    clearCanvas();
  }
}

function updateCursor(e: MouseEvent) {
  if (customCursor.value) {
    customCursor.value.style.transform = `translate(${e.clientX + 10}px, ${e.clientY + 10}px)`;
  }
}

onMounted(() => {
  if (!canvas.value) return;
  
  ctx = canvas.value.getContext('2d');
  resizeCanvas();

  canvas.value.addEventListener('mousedown', startDrawing);
  canvas.value.addEventListener('mousemove', draw);
  window.addEventListener('mouseup', stopDrawing);
  window.addEventListener('resize', resizeCanvas);
  window.addEventListener('mousemove', updateCursor);
  
  // Add click event to toggle drawing and clearing
  canvas.value.addEventListener('click', toggleDrawing);
});

onUnmounted(() => {
  if (canvas.value) {
    canvas.value.removeEventListener('mousedown', startDrawing);
    canvas.value.removeEventListener('mousemove', draw);
    // Remove click event
    canvas.value.removeEventListener('click', toggleDrawing);
  }
  window.removeEventListener('mouseup', stopDrawing);
  window.removeEventListener('resize', resizeCanvas);
  window.removeEventListener('mousemove', updateCursor);
});
</script>

<style scoped>
canvas {
  touch-action: none;
}
</style>
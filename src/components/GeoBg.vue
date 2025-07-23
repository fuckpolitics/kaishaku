<script setup>
import {ref} from "vue";
const shapes = ref([]);
const totalShapes = 30;
const generateShapes = () => {
  shapes.value = [];
  for (let i = 0; i < totalShapes; i++) {
    const width = 20 + Math.random() * 100; // больше
    const height = 10 + Math.random() * 60;
    const top = Math.random() * 100;
    const left = Math.random() * 100;
    const opacity = 0.15 + Math.random() * 0.3; // ярче фигуры
    const bgColors = ['#0f0', '#0ff', '#ff0', '#f0f'];
    const bgColor = bgColors[Math.floor(Math.random() * bgColors.length)];

    const duration = 15 + Math.random() * 25;

    const shapeType = Math.random() > 0.5 ? 'rect' : 'line';
    let style = {
      width: shapeType === 'rect' ? `${width}px` : `${width / 5}px`,
      height: shapeType === 'rect' ? `${height}px` : '3px',
      top: `${top}vh`,
      left: `${left}vw`,
      opacity,
      backgroundColor: bgColor,
      animationDuration: `${duration}s`,
      animationTimingFunction: 'ease-in-out',
      animationIterationCount: 'infinite',
      animationDirection: 'alternate',
      borderRadius: shapeType === 'rect' ? '8px' : '0',
      position: 'absolute',
      filter: 'drop-shadow(0 0 4px rgba(255,255,255,0.6))',
      mixBlendMode: 'screen',
      boxShadow: `0 0 12px ${bgColor}`
    };
    shapes.value.push({style});
  }
};
generateShapes();
</script>

<template>
  <div class="geo-bg">

    <div
        v-for="(shape, index) in shapes"
        :key="index"
        class="geo-shape"
        :style="shape.style"
    />
  </div>
</template>

<style scoped>
.geo-bg {
  position: fixed;
  inset: 0;
  z-index: 1;
  pointer-events: none;
  overflow: hidden;
}

.geo-shape {
  position: absolute;
  animation-name: geoMove;
  animation-timing-function: ease-in-out;
  animation-iteration-count: infinite;
  animation-direction: alternate;
  filter: drop-shadow(0 0 4px rgba(255, 255, 255, 0.6));
  mix-blend-mode: screen;
  border-radius: 8px;
  box-shadow: 0 0 12px currentColor;
}

@keyframes geoMove {
  0% {
    transform: translateY(0) translateX(0) scale(1);
  }
  50% {
    transform: translateY(-20px) translateX(20px) scale(1.1);
  }
  100% {
    transform: translateY(0) translateX(0) scale(1);
  }
}
</style>
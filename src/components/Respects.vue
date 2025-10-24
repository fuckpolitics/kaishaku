<template>
  <div class="respects-wrapper">
    <h1 class="respects-title">respects.</h1>
    <p class="respects-sub">names that burn through silence</p>

    <div class="respects-grid">
      <div
          v-for="(item, index) in respects"
          :key="index"
          class="respect-card"
          @click="toggleImage(index)"
      >
        <div class="respect-image">
          <transition name="flash" mode="out-in">
            <template v-if="!item.causeOfDeath || item.showImage">
              <img :src="item.image" :alt="item.name" key="img" />
            </template>
            <template v-else>
              <div class="cause-of-death" key="txt">{{ item.causeOfDeath }}</div>
            </template>
          </transition>
        </div>
        <div class="name">{{ item.name }}</div>
        <div class="yearsOfLife">{{ item.yearsOfLife }}</div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { reactive } from 'vue'

import lynch from '@/assets/img/respects/lynch.jpeg'
import lenin from '@/assets/img/respects/lenin.jpg'
import mishima from '@/assets/img/respects/mishima.webp'
import dudaev from '@/assets/img/respects/dudaev.jpeg'
import gandi from '@/assets/img/respects/gandi.jpg'
import west from '@/assets/img/respects/west.webp'

const respects = reactive([
  {
    name: "Дэвид Линч",
    yearsOfLife: "1946 — 2025",
    causeOfDeath: "растворён в эфире телевидения",
    image: lynch,
    showImage: false,
  },
  {
    name: "Владимир Ленин",
    yearsOfLife: "1870 — 1924",
    causeOfDeath: "перегрузка идеи",
    image: lenin,
    showImage: false,
  },
  {
    name: "Джохар Дудаев",
    yearsOfLife: "1944 — 1996",
    causeOfDeath: "ракета над головой",
    image: dudaev,
    showImage: false,
  },
  {
    name: "Махатма Ганди",
    yearsOfLife: "1869 — 1948",
    causeOfDeath: "пуля в спину",
    image: gandi,
    showImage: false,
  },
  {
    name: "Kanye West",
    yearsOfLife: "1977",
    image: west,
    showImage: true,
  },
  {
    name: "Юкио Мисима",
    yearsOfLife: "1925 — 1970",
    causeOfDeath: "ритуальное сэппуку",
    image: mishima,
    showImage: false,
  }
])

function toggleImage(index) {
  if (respects[index].causeOfDeath) {
    respects[index].showImage = true
  }
}
</script>

<style scoped>
.respects-wrapper {
  padding: 2rem;
  min-height: 100vh;
  overflow-y: auto;
  box-sizing: border-box;
}

.respects-title {
  font-size: 2.5rem;
  color: white;
  text-transform: lowercase;
  letter-spacing: 0.1em;
  margin-bottom: 0.5rem;
  filter: blur(0.5px);
  text-align: center;
}

.respects-sub {
  font-size: 1rem;
  color: #777;
  margin-bottom: 2rem;
  font-style: italic;
  text-align: center;
}

.respects-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
  gap: 2rem;
  width: 100%;
  max-width: 1000px;
  margin: 0 auto;
}

.respect-card {
  background: rgba(255, 255, 255, 0.05);
  border: 1px solid #444;
  padding: 1.5rem;
  border-radius: 10px;
  text-align: center;
  backdrop-filter: blur(3px);
  transition: transform 0.3s ease, filter 0.3s ease;
  cursor: pointer;
}

.respect-card:hover {
  transform: scale(1.03);
  filter: brightness(1.15) contrast(1.05);
}

.name {
  font-weight: bold;
  font-size: 1.2rem;
  color: white;
  margin-bottom: 0.5rem;
}

.yearsOfLife {
  font-size: 1.3rem;
  color: #aaa;
}

.respect-image {
  width: 100%;
  aspect-ratio: 1 / 1;
  overflow: hidden;
  border-radius: 6px;
  margin-bottom: 1rem;
  border: 1px solid #333;
  position: relative;
  background: #111;
  display: flex;
  align-items: center;
  justify-content: center;
}

.respect-image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  mix-blend-mode: lighten;
  opacity: 0.9;
  transition: opacity 0.3s ease;
}

.cause-of-death {
  color: #00ff88;
  font-size: 1rem;
  font-family: 'Courier New', monospace;
  font-style: italic;
  letter-spacing: 0.03em;
  padding: 0.5rem;
  text-align: center;
  line-height: 1.4;
  opacity: 0.95;
}

/* Transition effect — fast + clean */
.flash-enter-active,
.flash-leave-active {
  transition: all 0.3s ease;
}
.flash-enter-from,
.flash-leave-to {
  opacity: 0;
  transform: scale(0.95);
  filter: blur(2px);
}
.flash-enter-to,
.flash-leave-from {
  opacity: 1;
  transform: scale(1);
  filter: blur(0);
}

@media (max-width: 600px) {
  .respects-wrapper {
    padding: 1.5rem 1rem 3rem;
  }

  .respects-title {
    font-size: 1.8rem;
  }

  .respects-sub {
    font-size: 0.9rem;
    margin-bottom: 1.5rem;
  }

  .name {
    font-size: 1rem;
  }

  .yearsOfLife {
    font-size: 0.95rem;
  }

  .respect-card {
    padding: 1rem;
  }

  .respects-grid {
    gap: 1.2rem;
    grid-template-columns: repeat(auto-fit, minmax(160px, 1fr));
  }

  .respect-image {
    aspect-ratio: 4 / 5;
  }
}
</style>
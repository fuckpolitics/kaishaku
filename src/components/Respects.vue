<template>
  <div>
    <h1 class="respects-title">respects.</h1>
    <p class="respects-sub">names that burn through silence</p>

    <div class="respects-grid">
      <div
          v-for="(item, index) in respects"
          :key="index"
          class="respect-card"
      >
        <div class="respect-image">
          <img :src="item.image" :alt="item.name"/>
        </div>
        <div class="name">{{ item.name }}</div>
        <div class="yearsOfLife">{{ item.yearsOfLife }}</div>
      </div>
    </div>
  </div>
</template>

<script setup>
const respects = [
  {
    name: "Дэвид Линч",
    yearsOfLife: "1946 — 2025",
    glyph: "闇",
    image: '/kaishaku/src/assets/img/respects/lynch.jpeg'
  },
  {
    name: "Владимир Ленин",
    yearsOfLife: "1870 — 1924",
    glyph: "労",
    image: '/kaishaku/src/assets/img/respects/lenin.jpg'
  },
  {
    name: "Джохар Дудаев",
    yearsOfLife: "1944 — 1996",
    glyph: "狼",
    image: '/kaishaku/src/assets/img/respects/dudaev.jpeg'
  },
  {
    name: "Махатма Ганди",
    yearsOfLife: "1869 — 1948",
    glyph: "魂",
    image: '/kaishaku/src/assets/img/respects/gandi.jpg'
  },
  {
    name: "Kanye West",
    yearsOfLife: "1977",
    glyph: "神",
    image: '/kaishaku/src/assets/img/respects/west.webp'
  },
  {
    name: "Юкио Мисима",
    yearsOfLife: "1925-1970",
    glyph: "詩",
    image: '/kaishaku/src/assets/img/respects/mishima.webp'
  }
];
</script>

<style scoped>
.respects-title {
  font-size: 2.5rem;
  color: white;
  text-transform: lowercase;
  letter-spacing: 0.1em;
  margin-bottom: 0.5rem;
  filter: blur(0.5px);
}

.respects-sub {
  font-size: 1rem;
  color: #777;
  margin-bottom: 2rem;
  font-style: italic;
}

.respects-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
  gap: 2rem;
  width: 100%;
  max-width: 1000px;
}

.respect-card {
  background: rgba(255, 255, 255, 0.05);
  border: 1px solid #444;
  padding: 1.5rem;
  border-radius: 10px;
  text-align: center;
  backdrop-filter: blur(3px);
  transition: transform 0.3s ease, filter 0.3s ease;
  cursor: default;
}

.respect-card:hover {
  transform: scale(1.03);
  filter: brightness(1.2) contrast(1.1);
}

.glyph {
  font-size: 2rem;
  color: #888;
  margin-bottom: 0.5rem;
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
  filter: grayscale(1) contrast(1.4) brightness(0.9);
}

.respect-image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  mix-blend-mode: lighten;
  animation: flicker 2.5s infinite alternate;
  opacity: 0.9;
}

@keyframes flicker {
  0% {
    filter: brightness(0.9) contrast(1.2);
  }
  50% {
    filter: brightness(1.05) contrast(1.4) hue-rotate(2deg);
  }
  100% {
    filter: brightness(0.85) contrast(1.3) hue-rotate(-2deg);
  }
}

/* 📱 Mobile-specific adjustments */
@media (max-width: 600px) {
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
<script setup>
import { ref } from 'vue'

const albums = ref(
    Array.from({ length: 15 }, (_, i) => ({
      title: `Album ${i + 1}`,
      cover: `[${i + 1}]`,
    }))
)

const selectedAlbum = ref(null)
const albumList = ref(null)

const scrollLeft = () => {
  albumList.value.scrollLeft -= 150
}

const scrollRight = () => {
  albumList.value.scrollLeft += 150
}

const openAlbum = (album) => {
  selectedAlbum.value = album
}

const closeAlbum = () => {
  selectedAlbum.value = null
}
</script>

<template>
  <div class="music-section">
    <div class="music-frame">
      <div class="music-header">[ kaishaku.ninja music zone ]</div>

      <div class="album-scroll">
        <button class="scroll-btn" @click="scrollLeft">◀</button>

        <div class="album-list" ref="albumList">
          <div
              v-for="(album, index) in albums"
              :key="index"
              class="album"
              @click="openAlbum(album)"
          >
            <div class="album-cover">{{ album.cover }}</div>
            <div class="album-title">{{ album.title }}</div>
          </div>
        </div>

        <button class="scroll-btn" @click="scrollRight">▶</button>
      </div>

      <div class="music-footer">[ arrows to browse | click to open ]</div>
    </div>

    <div v-if="selectedAlbum" class="album-popup" @click="closeAlbum">
      <div class="popup-content">
        <h3>{{ selectedAlbum.title }}</h3>
        <p>[placeholder info]</p>
      </div>
    </div>
  </div>
</template>

<style scoped>
.music-section {
  z-index: 20;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  color: #0f0;
  text-shadow: 0 0 3px #0f0;
  font-family: 'Visitor', monospace;
  width: 100%;
  height: 100%;
  overflow: visible;
}

.music-frame {
  width: 90%;
  max-width: 800px;
  border: 2px solid #0f0;
  padding: 1rem;
  background-color: #000;
  overflow: visible;
  position: relative;
}

.music-header,
.music-footer {
  text-align: center;
  margin-bottom: 1rem;
  font-size: 0.9rem;
  border-bottom: 1px dashed #0f0;
  padding-bottom: 0.5rem;
}

.album-scroll {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 1rem;
}

.album-list {
  display: flex;
  gap: 1rem;
  overflow-x: auto;
  scrollbar-width: none;
  -ms-overflow-style: none;
  padding: 0.5rem;
  scroll-behavior: smooth;
  max-width: 80%;
}

.album-list::-webkit-scrollbar {
  display: none;
}

.album {
  flex: 0 0 auto;
  width: 100px;
  height: 120px;
  background-color: #111;
  border: 1px solid #0f0;
  text-align: center;
  cursor: pointer;
  padding: 0.5rem;
}

.album-cover {
  height: 60px;
  background: #0f0;
  margin-bottom: 0.5rem;
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: bold;
}

.album-title {
  font-size: 0.75rem;
}

.scroll-btn {
  background-color: #000;
  color: #0f0;
  border: 1px solid #0f0;
  font-size: 1.2rem;
  width: 2rem;
  height: 2rem;
  cursor: pointer;
}

.album-popup {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  background-color: rgba(0, 0, 0, 0.85);
  display: flex;
  justify-content: center;
  align-items: center;
  color: #0f0;
  z-index: 9999;
  pointer-events: auto;
}

.popup-content {
  background: #000;
  padding: 2rem;
  border: 2px solid #0f0;
  text-align: center;
  font-family: 'Visitor', monospace;
}
</style>
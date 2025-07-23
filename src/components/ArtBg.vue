<template>
  <div class="art-bg">
    <div
        v-for="(line, i) in renderedLines"
        :key="i"
        class="art-line"
        :class="{ 'no-flicker': currentLanguage !== 'arabic' }"
        :style="artStyles[i]"
    >
      {{ line }}
    </div>
  </div>
</template>

<script setup>
import { computed, watch, onMounted, ref } from 'vue';
const props = defineProps({
  currentLanguage: String,
  languagePhrases: Object,
  totalArtLines: {
    type: Number,
    default: 50
  }
});

const renderedLines = ref([]);
const artStyles = ref([]);

function generateArtLines() {
  const phrases = props.languagePhrases[props.currentLanguage] || [];
  renderedLines.value = [];
  artStyles.value = [];

  for (let i = 0; i < props.totalArtLines; i++) {
    const randomLine = phrases[Math.floor(Math.random() * phrases.length)];
    renderedLines.value.push(randomLine);
    artStyles.value.push(generateStaticStyle());
  }
}

function generateStaticStyle() {
  const top = Math.random() * 100;
  const left = Math.random() * 100;
  const rotation = (Math.random() - 0.5) * 60;
  const size = 1.5 + Math.random() * 3;
  const opacity = 0.125 + Math.random() * 0.175;
  const duration = 30 + Math.random() * 50;

  let fontFamily = '';
  switch (props.currentLanguage) {
    case 'arabic':
      fontFamily = "'Scheherazade', serif";
      break;
    case 'greek':
      fontFamily = "'New Athena Unicode', serif";
      break;
    case 'mongolian':
      fontFamily = "'Mongolian Baiti', serif";
      break;
    case 'japanese':
      fontFamily = "'MS Mincho', 'Hiragino Mincho Pro', serif";
      break;
    case 'sanskrit':
      fontFamily = "'Sanskrit Text', serif";
      break;
    default:
      fontFamily = "'Scheherazade', serif";
  }

  return {
    top: `${top}vh`,
    left: `${left}vw`,
    transform: `rotate(${rotation}deg)`,
    fontSize: `${size}rem`,
    opacity,
    animationDuration: `${duration}s`,
    fontFamily,
    writingMode: props.currentLanguage === 'mongolian' ? 'vertical-rl' : 'horizontal-tb',
    color: `rgba(255, 255, 255, ${opacity + 0.2})`,
    textShadow: `0 0 8px rgba(255, 255, 255, ${opacity + 0.4})`
  };
}

watch(() => props.currentLanguage, generateArtLines, { immediate: true });
</script>
<template>
  <div class="app" :class="{ fading: fadeOut, crt: activeSection }">
    <!-- Яркий артхаусный геометрический фон -->
    <div class="geo-bg">
      <div
          v-for="(shape, index) in shapes"
          :key="index"
          class="geo-shape"
          :style="shape.style"
      />
    </div>

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

    <div v-if="!activeSection" class="content">
      <h1 class="title" @mouseenter="glitch = true" @animationend="glitch = false" :class="{ glitch }">
        kaishaku.ninja
      </h1>
      <p class="subtitle">fragmented presence in digital shadows</p>

      <nav class="nav">
        <ul>
          <li v-for="section in sections" :key="section">
            <a href="#" @click.prevent="goTo(section)">{{ section }}</a>
          </li>
        </ul>
      </nav>
    </div>

    <div v-else class="section">
      <h2 class="section-title">{{ activeSection }}</h2>
      <p class="section-text">[content of {{ activeSection }} will go here]</p>
      <a href="#" class="back" @click.prevent="goBack">back</a>
    </div>

    <div class="cursor" :style="{ left: cursorX + 'px', top: cursorY + 'px' }" />
  </div>
</template>

<script>
export default {
  data() {
    return {
      glitch: false,
      fadeOut: false,
      activeSection: null,
      cursorX: 0,
      cursorY: 0,
      sections: ['music', 'software', 'portfolio', 'thoughts', 'respects'],
      artStyles: [],
      renderedLines: [],
      totalArtLines: 50,
      languagePhrases: {
        arabic: [
          "وَجَدْتُ رَبِّي فِي قَلْبِي",
          "الصمتُ حكمة",
          "الضوء في الداخل",
          "من عرف نفسه فقد عرف ربه",
          "في السكونِ حياة",
          "الحقيقة مثل المرآة",
          "الكلُّ فانٍ إلا وجهه"
        ],
        greek: [
          "Γνῶθι σεαυτόν",
          "Πάντα ῥεῖ",
          "Ἡ σιωπὴ φωνὴ τῆς ἀληθείας",
          "Ἐν τῷ βυθῷ τῆς ψυχῆς ἡ ἀλήθεια",
          "Ὁ βίος βραχύς, ἡ δὲ τέχνη μακρή",
          "Τὸ γνῶναι ἑαυτὸν μέγιστον ἐπιστήμης",
          "Ἡ ἁπλότης ὑψίστη σοφία"
        ],
        mongolian: [
          "ᠮᠢᠨᠢ ᠮᠡᠨᠳᠡᠯᠡᠬᠦ ᠦᠭᠡᠢ ᠪᠢᠴᠢᠭ ᠦᠭᠡᠢ",
          "ᠠᠮᠠᠨ ᠰᠠᠨᠠᠯ ᠦᠭᠡᠢ ᠪᠠᠢᠨᠠ",
          "ᠨᠢᠭᠡ ᠶᠢᠨ ᠭᠡᠷᠡᠯ ᠳ᠋ᠡᠭᠡᠨ ᠬᠠᠷᠢᠴᠠᠭᠠᠷᠠᠢ",
          "ᠪᠦᠷᠢᠨ ᠰᠡᠳᠭᠢᠯ ᠦᠨᠡᠨ ᠰᠠᠶ᠋ᠢᠨ",
          "ᠴᠢᠨᠤ ᠰᠢᠲᠤ ᠪᠠᠷᠠᠭᠤᠨ ᠦᠭᠡᠢ",
          "ᠦᠯᠦ ᠶᠢᠨ ᠰᠠᠨᠠᠯ ᠨᠢ ᠦᠨᠡᠨ ᠰᠠᠶ᠋ᠢᠨ",
          "ᠨᠢᠭᠡ ᠶᠢᠨ ᠰᠠᠨᠠᠯ ᠨᠢ ᠪᠤᠰᠤᠭᠠᠨ"
        ],
        japanese: [
          "己を知る者は智なり",
          "無為自然",
          "静かなる心は鏡の如し",
          "一即一切、一切即一",
          "心の闇を照らす光",
          "無の境地に真実あり",
          "変化こそ不変の真理"
        ],
        sanskrit: [
          "तत्त्वमसि",
          "अहं ब्रह्मास्मि",
          "सत्यमेव जयते",
          "वसुधैव कुटुम्बकम्",
          "असतो मा सद्गमय",
          "योगः कर्मसु कौशलम्",
          "शान्तिः शान्तिः शान्तिः"
        ]
      },
      currentLanguage: 'arabic',

      shapes: [],
      totalShapes: 30,
    };
  },
  created() {
    this.generateArtLines();
    this.generateShapes();
  },
  mounted() {
    window.addEventListener('mousemove', this.updateCursor);
  },
  beforeDestroy() {
    window.removeEventListener('mousemove', this.updateCursor);
  },
  methods: {
    updateCursor(e) {
      this.cursorX = e.clientX;
      this.cursorY = e.clientY;
    },
    goTo(section) {
      this.fadeOut = true;
      setTimeout(() => {
        this.activeSection = section;
        switch (section) {
          case 'music':
            this.currentLanguage = 'mongolian';
            break;
          case 'software':
            this.currentLanguage = 'greek';
            break;
          case 'portfolio':
            this.currentLanguage = 'japanese';
            break;
          case 'thoughts':
            this.currentLanguage = 'sanskrit';
            break;
          case 'respects':
            this.currentLanguage = 'arabic';
            break;
          default:
            this.currentLanguage = 'arabic';
        }
        this.generateArtLines();
        this.fadeOut = false;
      }, 800);
    },
    goBack() {
      this.fadeOut = true;
      setTimeout(() => {
        this.activeSection = null;
        this.currentLanguage = 'arabic';
        this.generateArtLines();
        this.fadeOut = false;
      }, 800);
    },
    generateArtLines() {
      this.renderedLines = [];
      this.artStyles = [];
      const phrases = this.languagePhrases[this.currentLanguage];

      for (let i = 0; i < this.totalArtLines; i++) {
        const randomLine = phrases[Math.floor(Math.random() * phrases.length)];
        this.renderedLines.push(randomLine);
        this.artStyles.push(this.generateStaticStyle());
      }
    },
    generateStaticStyle() {
      const top = Math.random() * 100;
      const left = Math.random() * 100;
      const rotation = (Math.random() - 0.5) * 60;
      const size = 1.5 + Math.random() * 3;
      const opacity = 0.25 + Math.random() * 0.35; // ярче
      const duration = 30 + Math.random() * 50;

      let fontFamily = '';
      switch (this.currentLanguage) {
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
        writingMode: this.currentLanguage === 'mongolian' ? 'vertical-rl' : 'horizontal-tb',
        color: `rgba(255, 255, 255, ${opacity + 0.2})`, // белый текст с прозрачностью
        textShadow: `0 0 8px rgba(255, 255, 255, ${opacity + 0.4})`
      };
    },
    generateShapes() {
      this.shapes = [];
      for (let i = 0; i < this.totalShapes; i++) {
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

        this.shapes.push({ style });
      }
    }
  }
};
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Scheherazade:wght@400;700&display=swap');

html, body {
  margin: 0;
  padding: 0;
  background: #000;
  font-family: 'Courier New', monospace;
  height: 100%;
  overflow: hidden;
  cursor: none;
}

.app {
  width: 100%;
  height: 100vh;
  background: #000;
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
  transition: background 0.8s ease;
  overflow: hidden;
}

.app.fading {
  background: #000;
  opacity: 0;
  transition: opacity 0.8s ease;
}

.app.crt {
  background: #000;
  position: relative;
}

.geo-bg {
  position: fixed;
  inset: 0;
  z-index: 0;
  pointer-events: none;
  overflow: hidden;
}

.geo-shape {
  position: absolute;
  animation-name: geoMove;
  animation-timing-function: ease-in-out;
  animation-iteration-count: infinite;
  animation-direction: alternate;
  filter: drop-shadow(0 0 4px rgba(255,255,255,0.6));
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

.art-bg {
  position: fixed;
  inset: 0;
  z-index: 1;
  pointer-events: none;
  overflow: hidden;
  direction: rtl;
}

.art-line {
  position: absolute;
  white-space: nowrap;
  animation-name: floatyMove;
  animation-duration: 20s;
  animation-timing-function: ease-in-out;
  animation-iteration-count: infinite;
  animation-direction: alternate;
  user-select: none;
  color: white; /* по умолчанию */
  text-shadow: 0 0 6px #fff;
}

.art-line[style*="vertical-rl"] {
  text-orientation: upright;
  letter-spacing: 0.5em;
}

@keyframes floatyMove {
  0% {
    transform: translateY(0) scale(1);
  }
  100% {
    transform: translateY(-10px) scale(1.03);
  }
}

.content {
  z-index: 10;
  text-align: center;
}

.title {
  font-size: 3rem;
  color: #fff;
  text-transform: lowercase;
  animation: fadeIn 1.5s ease;
  text-shadow:
      0 0 8px #fff,
      0 0 20px #0f0,
      0 0 30px #0f0;
}

.title.glitch {
  animation: glitch 0.6s steps(2, end);
}

.subtitle {
  font-size: 1rem;
  color: #ccc;
  margin-top: 0.5rem;
  text-shadow: 0 0 4px #0f0;
}

.nav ul {
  display: flex;
  gap: 2rem;
  list-style: none;
  padding: 0;
  justify-content: center;
  margin-top: 2rem;
}

.nav a {
  color: #aaa;
  text-transform: lowercase;
  text-decoration: none;
  font-size: 0.85rem;
  transition: color 0.3s ease;
}

.nav a:hover {
  color: #0f0;
  text-shadow: 0 0 6px #0f0;
}

.section {
  z-index: 20;
  color: #0f0;
  text-align: center;
  width: 100%;
}

.section-title {
  font-size: 2rem;
  text-transform: lowercase;
  margin-top: 2rem;
  text-shadow: 0 0 1px #0f0, 0 0 2px #0f0;
}

.section-text {
  margin-top: 1rem;
  font-size: 0.95rem;
  color: #0f0;
  opacity: 0.9;
  text-shadow: 0 0 1px #0f0;
}

.back {
  display: inline-block;
  margin-top: 2rem;
  color: #0f0;
  text-decoration: none;
  font-size: 0.9rem;
  border: 1px dashed #0f0;
  padding: 0.3rem 0.7rem;
  text-transform: lowercase;
  transition: background 0.3s ease;
}

.back:hover {
  background: rgba(0, 255, 0, 0.1);
}

.cursor {
  position: fixed;
  width: 6px;
  height: 6px;
  background-color: #ccc;
  border-radius: 50%;
  pointer-events: none;
  z-index: 9999;
  box-shadow: 0 0 8px rgba(255,255,255,0.6);
  transform: translate(-50%, -50%);
}

@keyframes fadeIn {
  from { opacity: 0; transform: translateY(10px); }
  to   { opacity: 1; transform: translateY(0); }
}

@media (max-width: 600px) {
  .title { font-size: 2rem; }
  .subtitle { font-size: 0.85rem; }
  .nav ul { flex-wrap: wrap; gap: 1rem; }
  .section-title { font-size: 1.4rem; }
}
</style>
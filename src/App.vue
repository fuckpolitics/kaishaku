<template>
  <div v-if="loading" class="intro-screen">
    <div class="glyph-sequence">
      <span v-for="(glyph, index) in introGlyphs" :key="index" class="glyph"
            :style="{ animationDelay: `${index * 0.15}s` }">
        {{ glyph }}
      </span>
    </div>
  </div>
  <div class="app" :class="{ fading: fadeOut, crt: activeSection }">
    <GeoBg/>

    <ArtBg
        :currentLanguage="currentLanguage"
        :languagePhrases="languagePhrases"
        :totalArtLines="totalArtLines"
        :tiltX="tiltX"
        :tiltY="tiltY"
    />

    <div v-if="!activeSection" class="page-container">
      <div class="content">
        <h1 class="title" @mouseenter="glitch = true" @animationend="glitch = false" :class="{ glitch }">
          kaishaku.ninja
        </h1>
        <sigil-nav 
          :sections="sections" 
          :active-section="activeSection"
          @navigate="goTo"
        />
      </div>
    </div>

    <div v-else class="section">
      <music v-if="activeSection === 'music'"/>
      <respects v-else-if="activeSection === 'respects'"/>
      <div v-else>
        <h2 class="section-title">{{ activeSection }}</h2>
        <p class="section-text">[content of {{ activeSection }} will go here]</p>
      </div>
      <a href="#" class="back" @click.prevent="goBack">back</a>
    </div>
    <div class="kaonashi" :style="kaonashiStyle"/>
  </div>
</template>

<script>
import Respects from "@/components/Respects.vue";
import Music from "@/components/Music.vue";
import GeoBg from "@/components/GeoBg.vue";
import ArtBg from "@/components/ArtBg.vue";
import SigilNav from "@/components/SigilNav.vue";

export default {
  components: { ArtBg, GeoBg, Music, Respects, SigilNav },
  data() {
    return {
      loading: true,
      introGlyphs: [
        '𒀭', '𓂀', 'Ϟ', 'ᚱ', '𐌔', '߷', 'ᛉ', '҂', '𐎔', '𓊽', '☥', '𐎟', '𐤀', 'ᛏ', '⸸'
      ],
      glitch: false,
      crtTransitioning: false,
      fadeOut: false,
      activeSection: null,
      cursorX: 0,
      cursorY: 0,
      tiltX: 0,
      tiltY: 0,
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
    };
  },
  created() {
    this.generateArtLines();
  },
  mounted() {
    window.addEventListener('mousemove', this.updateCursor);
    window.addEventListener('deviceorientation', this.handleTilt);
    this.startIntro();
  },
  beforeDestroy() {
    window.removeEventListener('mousemove', this.updateCursor);
    window.removeEventListener('deviceorientation', this.handleTilt);
  },
  methods: {
    startIntro() {
      setTimeout(() => {
        this.loading = false;
      }, 4500);
    },
    updateCursor(e) {
      this.cursorX = e.clientX;
      this.cursorY = e.clientY;
    },
    handleTilt(event) {
      const { beta, gamma } = event;
      this.tiltX = (gamma / 45) * 10; // максимум ±10
      this.tiltY = (beta / 45) * 10;
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
        this.crtTransitioning = true;
        setTimeout(() => {
          this.activeSection = section;
          this.crtTransitioning = false;
        }, 1200);
        this.generateArtLines();
        this.fadeOut = false;
      }, 800);
    },
    goBack() {
      this.crtTransitioning = true;
      setTimeout(() => {
        this.activeSection = null;
        this.currentLanguage = 'arabic';
        this.generateArtLines();
        this.crtTransitioning = false;
      }, 1200);
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
      const opacity = 0.125 + Math.random() * 0.175;
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
        color: `rgba(255, 255, 255, ${opacity + 0.2})`,
        textShadow: `0 0 8px rgba(255, 255, 255, ${opacity + 0.4})`
      };
    },
  },
  computed: {
    kaonashiStyle() {
      return {
        transform: `translate(${this.tiltX * 0.5}px, ${this.tiltY * 0.5}px)`
      };
    }
  }
};
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Scheherazade:wght@400;700&display=swap');

@font-face {
  font-family: 'Visitor';
  src: url('@/assets/fonts/visitor2.ttf') format('truetype');
  font-weight: normal;
  font-style: normal;
}

html, body {
  margin: 0;
  padding: 0;
  background: #000;
  font-family: 'Visitor', monospace;
  min-height: 100vh;
  overflow-x: hidden;
  overflow-y: auto;
  //display: grid;
  //cursor: none;
}

.app {
  width: 100%;
  min-height: 100vh;
  background: #000;
  position: relative;
  transition: background 0.8s ease;
  overflow: hidden;
}

.page-container {
  position: relative;
  width: 100%;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 2rem;
  box-sizing: border-box;
  z-index: 2;
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
  text-align: center;
  width: 100%;
  max-width: 1200px;
  margin: 0 auto;
  padding: 2rem;
  box-sizing: border-box;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 2rem;
}

.title {
  font-size: 3rem;
  color: #fff;
  text-transform: lowercase;
  animation: fadeIn 1.5s ease;
  text-shadow: 0 0 8px #fff,
  0 0 20px #0f0,
  0 0 30px #0f0;
}

.title.glitch {
  animation: glitch 0.6s steps(2, end);
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
  color: #e0ffe0;
  text-transform: lowercase;
  text-decoration: none;
  font-size: 0.95rem;
  transition: all 0.3s ease;
  text-shadow: 0 0 2px #0f0,
  0 0 4px #0f0,
  0 0 6px #0f0;
  letter-spacing: 0.05em;
  position: relative;
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

@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(10px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@media (max-width: 600px) {
  .title {
    font-size: 2rem;
  }

  .nav ul {
    flex-wrap: wrap;
    gap: 1rem;
  }

  .section-title {
    font-size: 1.4rem;
  }
}

@media {
  #app {
    display: flex;
    justify-content: center;
    padding: 0 2rem;
  }
}

.intro-screen {
  position: fixed;
  inset: 0;
  background-color: #000;
  z-index: 99999;
  display: flex;
  justify-content: center;
  align-items: center;
  animation: screenFlash 0.8s ease-in-out infinite alternate;
}

.glyph-sequence {
  display: flex;
  flex-wrap: wrap;
  max-width: 80vw;
  justify-content: center;
  font-size: 3rem;
  font-family: 'Visitor', monospace;
  color: #0f0;
  text-shadow: 0 0 10px #0f0, 0 0 20px #0f0;
}

.glyph {
  opacity: 0;
  animation: glyphFade 0.6s ease forwards;
  margin: 0.2rem;
}

@keyframes glyphFade {
  to {
    opacity: 1;
    transform: scale(1.2);
  }
}

@keyframes screenFlash {
  0% {
    background-color: #000;
  }
  100% {
    background-color: #020;
  }
}

.intro-screen::after {
  content: '';
  position: absolute;
  inset: 0;
  background: white;
  opacity: 0;
  animation: flashOut 4s ease forwards;
  animation-delay: 3.8s;
  pointer-events: none;
}

@keyframes flashOut {
  0% {
.kaonashi {
  position: fixed;
  bottom: 0;
  right: 2vw;
  width: 360px;
  aspect-ratio: 2 / 3; /* Примерно как у Каонаси */
  background-image: url('./assets/img/no-face.png');
  background-size: contain;
  background-repeat: no-repeat;
  background-position: center;
  opacity: 0.4;
  animation: floaty 6s ease-in-out infinite;
  z-index: 0;
  filter: grayscale(1) contrast(1.2);
}

@keyframes floaty {
  0% {
    transform: translateY(0px) scale(1);
    opacity: 0.4;
  }
  50% {
    transform: translateY(-10px) scale(1.03);
    opacity: 0.6;
  }
  100% {
    transform: translateY(0px) scale(1);
    opacity: 0.4;
  }
}


.art-line {
  transition: transform 0.2s ease-out;
}

.kaonashi {
  transition: transform 0.3s ease-out;
}}}

</style>


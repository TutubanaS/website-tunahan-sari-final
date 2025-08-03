<template>
  <div id="app" @mousemove="handleMouseMove">
    <!-- Rotating Gradient Background -->
    <div class="gradient-background" :style="gradientStyle"></div>
    
    <!-- 3D Cube -->
    <div class="cube-container" :style="cubeContainerStyle">
      <div class="cube" :style="cubeStyle">
        <div class="cube-face front"></div>
        <div class="cube-face back"></div>
        <div class="cube-face right"></div>
        <div class="cube-face left"></div>
        <div class="cube-face top"></div>
        <div class="cube-face bottom"></div>
      </div>
    </div>
    
    <!-- Loading Screen -->
    <transition name="fade">
      <div v-if="isLoading" class="loading-screen">
        <div class="loading-content">
          <div class="loading-name">Tunahan Sarı</div>
          <div class="loading-title">
            AI & Software<br />
            Engineer
          </div>
        </div>
      </div>
    </transition>
    
    <!-- Content -->
    <transition name="fade">
      <div v-if="showContent" class="container">
        <AppHeader class="header-component" @section-selected="setActiveSection" />
        <div class="content">
          <transition name="fade" mode="out-in">
            <component :is="currentComponent" class="main-container" />
          </transition>
        </div>
      </div>
    </transition>
  </div>
</template>

<script>
import AppHeader from './components/AppHeader.vue';
import AppHome from './components/AppHome.vue';
import AppProjects from './components/AppProjects.vue';
import AppSkills from './components/AppSkills.vue';
import AppExperience from './components/AppExperience.vue';
import AppContactFaq from './components/AppContact.vue';

function hexToRgb(hex) {
  const cleaned = hex.replace('#', '');
  const bigint = parseInt(cleaned, 16);
  if (cleaned.length === 6) {
    return {
      r: (bigint >> 16) & 255,
      g: (bigint >> 8) & 255,
      b: bigint & 255
    };
  } else if (cleaned.length === 3) {
    return {
      r: ((bigint >> 8) & 15) * 17,
      g: ((bigint >> 4) & 15) * 17,
      b: (bigint & 15) * 17
    };
  }
  return { r: 0, g: 0, b: 0 };
}

function rgbToHex({ r, g, b }) {
  const clamp = (v) => Math.max(0, Math.min(255, Math.round(v)));
  return (
    '#' +
    ((1 << 24) + (clamp(r) << 16) + (clamp(g) << 8) + clamp(b))
      .toString(16)
      .slice(1)
  );
}

function lerp(a, b, t) {
  return a + (b - a) * t;
}

function interpolateColor(c1, c2, t) {
  return {
    r: lerp(c1.r, c2.r, t),
    g: lerp(c1.g, c2.g, t),
    b: lerp(c1.b, c2.b, t)
  };
}

export default {
  name: 'App',
  components: {
    AppHeader,
    AppHome,
    AppProjects,
    AppSkills,
    AppExperience,
    AppContactFaq
  },
  data() {
    return {
      isLoading: true,
      showContent: false,
      loadingGradient: null, // Will store random gradient for loading screen
      activeSection: 'home',
      mouseX: 0,
      mouseY: 0,
      windowWidth: window.innerWidth,
      windowHeight: window.innerHeight,
      baseRotateX: -25,
      baseRotateYAngle: 0,
      baseRotateY: 0,
      currentRotateX: -25,
      currentRotateY: 0,
      cubePositionX: 0,
      cubePositionY: 0,
      moveAngle: 0,
      lastMouseMove: Date.now(),
      cubeSize: 600, // Increased default cube size
      spinRemaining: 0,
      spinOffsetY: 0,
      spinSpeed: 1.8,
      contentLoadSpinSpeed: 3.0, // Faster spin for content load animation

      // gradient crossfade state
      gradientPresets: {
        home: ['#1c2438', '#0d0f16', '#2f3152'],
        projects: ['#1f332d', '#0e100f', '#274843'],
        skills: ['#2d1f4a', '#120f13', '#563b7a'],
        experience: ['#1f4143', '#0c1f21', '#2c5558'],
        contact: ['#4a2f1f', '#1a0f0f', '#6b5a3b']
      },
      baseGradient: [], // initialized below
      targetGradient: null,
      fadeProgress: 1,
      fading: false,
      gradientFadeRaf: null
    };
  },
  mounted() {
    this.updateWindowSize();
    window.addEventListener('resize', this.updateWindowSize);
    
    // Select a random gradient for loading screen
    const sections = Object.keys(this.gradientPresets);
    const randomSection = sections[Math.floor(Math.random() * sections.length)];
    this.loadingGradient = this.gradientPresets[randomSection].slice();
    console.log(`Loading with random section: ${randomSection}`);
    
    // initialize baseGradient immediately (avoid null)
    this.baseGradient = this.loadingGradient.slice();
    this.animateBaseRotation(performance.now());
    
    // Show loading screen for 3 seconds
    setTimeout(() => {
      this.isLoading = false;
      
      // Transition to home gradient when loading screen disappears
      this.startGradientCrossfade(this.gradientPresets.home);
      
      // Load main content 1 second after loading screen disappears
      setTimeout(() => {
        this.showContent = true;
        // Give the cube a nice spin when content appears
        this.startSpin(1, true); // Use enhanced spin animation
      }, 1000);
    }, 3000);
  },
  beforeUnmount() {
    window.removeEventListener('resize', this.updateWindowSize);
    if (this.gradientFadeRaf) cancelAnimationFrame(this.gradientFadeRaf);
  },
  watch: {
    activeSection(newVal, oldVal) {
      if (newVal === oldVal) return;
      const next = this.gradientPresets[newVal] || this.gradientPresets.home;
      this.startGradientCrossfade(next);
    }
  },
  computed: {
    currentComponent() {
      switch (this.activeSection) {
        case 'home': return AppHome;
        case 'projects': return AppProjects;
        case 'skills': return AppSkills;
        case 'experience': return AppExperience;
        case 'contact': return AppContactFaq;
        default: return AppHome;
      }
    },
    cubeStyle() {
      const scale = 1 + Math.sin(this.moveAngle * 0.5) * 0.01;
      return {
        transform: `rotateX(${this.currentRotateX.toFixed(3)}deg) rotateY(${this.currentRotateY.toFixed(3)}deg) scale(${scale.toFixed(3)})`,
        width: `${this.cubeSize}px`,
        height: `${this.cubeSize}px`,
        '--half': `${this.cubeSize / 2}px`
      };
    },
    cubeContainerStyle() {
      // Scale the cube position based on screen size
      const positionScale = this.cubeSize / 400;
      const scaledX = this.cubePositionX * positionScale;
      const scaledY = this.cubePositionY * positionScale;
      
      return {
        transform: `translate(calc(-50% + ${scaledX}px), calc(-50% + ${scaledY}px))`
      };
    },
    gradientStyle() {
      const fallback = this.gradientPresets.home;
      let colors = fallback;

      if (this.fading && Array.isArray(this.baseGradient) && Array.isArray(this.targetGradient)) {
        const t = 1 - Math.pow(1 - Math.min(1, this.fadeProgress), 3);
        colors = this.baseGradient.map((from, i) => {
          const to = this.targetGradient[i] || from;
          const c1 = hexToRgb(from);
          const c2 = hexToRgb(to);
          const interp = interpolateColor(c1, c2, t);
          return rgbToHex(interp);
        });
      } else if (Array.isArray(this.baseGradient) && this.baseGradient.length === 3) {
        colors = this.baseGradient;
      }

      return {
        background: `linear-gradient(135deg, ${colors[0]}, ${colors[1]}, ${colors[2]})`,
        backgroundSize: '300% 300%',
        animation: 'gradientShift 40s ease infinite',
        transition: 'background 0s'
      };
    }
  },
  methods: {
    startGradientCrossfade(newGradient) {
      if (!Array.isArray(newGradient) || newGradient.length !== 3) return;
      // cancel ongoing fade to avoid race
      if (this.gradientFadeRaf) {
        cancelAnimationFrame(this.gradientFadeRaf);
        this.gradientFadeRaf = null;
      }

      // set up new fade
      this.targetGradient = newGradient.slice();
      this.fadeProgress = 0;
      this.fading = true;
      const duration = 600;
      const start = performance.now();

      const step = (now) => {
        const elapsed = now - start;
        this.fadeProgress = Math.min(1, elapsed / duration);
        if (this.fadeProgress < 1) {
          this.gradientFadeRaf = requestAnimationFrame(step);
        } else {
          // finalize safely only if targetGradient exists
          if (Array.isArray(this.targetGradient)) {
            this.baseGradient = this.targetGradient.slice();
          }
          this.targetGradient = null;
          this.fading = false;
          this.fadeProgress = 1;
          this.gradientFadeRaf = null;
        }
      };

      this.gradientFadeRaf = requestAnimationFrame(step);
    },
    setActiveSection(newSection) {
      const order = ['home', 'projects', 'skills', 'experience', 'contact'];
      const oldIndex = order.indexOf(this.activeSection);
      const newIndex = order.indexOf(newSection);
      if (newIndex === -1) {
        this.activeSection = newSection;
        return;
      }
      if (newIndex !== oldIndex) {
        const direction = newIndex < oldIndex ? -1 : 1;
        this.startSpin(direction);
      }
      this.activeSection = newSection;
    },
    startSpin(direction, isContentLoad = false) {
      // Regular spin for navigation
      if (!isContentLoad) {
        this.spinRemaining += direction * (3 * 75);
        return;
      }
      
      // Enhanced spin for content load animation
      this.spinRemaining += direction * (3 * 75); 
    },
    handleMouseMove(event) {
      this.mouseX = event.clientX;
      this.mouseY = event.clientY;
      this.lastMouseMove = Date.now();
    },
    updateWindowSize() {
      this.windowWidth = window.innerWidth;
      this.windowHeight = window.innerHeight;
      
      // Responsive cube sizing based on both screen dimensions - bigger cube
      const minDimension = Math.min(this.windowWidth, this.windowHeight);
      this.cubeSize = Math.min(600, Math.max(200, minDimension * 0.5));
    },
    lerp(a, b, t) {
      return a + (b - a) * t;
    },
    animateBaseRotation(lastTimestamp) {
      const maxYaw = 25;
      const loop = (timestamp) => {
        const dt = Math.min(50, timestamp - lastTimestamp);
        lastTimestamp = timestamp;
        const now = Date.now();
        const idleTime = now - this.lastMouseMove;
        const isIdle = idleTime > 2000;
        const floatSpeed = isIdle ? 0.002 : 0.005;
        const yawSpeed = isIdle ? 0.002 : 0.008;
        this.baseRotateYAngle = (this.baseRotateYAngle + yawSpeed) % (Math.PI * 2);
        this.baseRotateY = Math.sin(this.baseRotateYAngle) * maxYaw;
        this.moveAngle = (this.moveAngle + floatSpeed) % (Math.PI * 2);
        
        // Base movement values
        const baseMovementX = 25;
        const baseMovementY = 15;
        
        // Scale movement based on screen size
        const movementScale = Math.min(1, this.windowWidth / 1200);
        this.cubePositionX = Math.sin(this.moveAngle) * baseMovementX * movementScale;
        this.cubePositionY = Math.sin(this.moveAngle * 1.5) * baseMovementY * movementScale;
        if (this.spinRemaining !== 0) {
          // Use faster spin speed for larger spin values (content load animation)
          const useSpeed = Math.abs(this.spinRemaining) > 500 ? this.contentLoadSpinSpeed : this.spinSpeed;
          const applyMagnitude = Math.min(Math.abs(this.spinRemaining), useSpeed * dt);
          const apply = Math.sign(this.spinRemaining) * applyMagnitude;
          this.spinOffsetY += apply;
          this.spinRemaining -= apply;
        }
        const targetRotateX = this.baseRotateX;
        const mouseInfluenceY = (this.mouseX - this.windowWidth / 2) / 25;
        const targetRotateY = this.baseRotateY + mouseInfluenceY + this.spinOffsetY;
        this.currentRotateX = this.lerp(this.currentRotateX, targetRotateX, 0.1);
        this.currentRotateY = this.lerp(this.currentRotateY, targetRotateY, 0.1);
        requestAnimationFrame(loop);
      };
      requestAnimationFrame(loop);
    }
  }
};
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Inter:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&display=swap');

html, body, #app {
  height: 100%;
  margin: 0;
  padding: 0;
  font-family: 'Inter', sans-serif;
}

body {
  overflow: hidden;
  background: #0f0f14;
}

#app {
  position: relative;
  width: 100%;
  height: 100%;
  overflow: hidden;
}

.gradient-background {
  position: absolute;
  top: 0;
  left: 0;
  width: 150%;
  height: 150%;
  background-size: 300% 300%;
  z-index: -3;
}

@keyframes gradientShift {
  0% { background-position: 0% 50%; }
  50% { background-position: 100% 50%; }
  100% { background-position: 0% 50%; }
}

.cube-container {
  position: absolute;
  top: 50%;
  left: 50%;
  perspective: calc(2400px + 10vw); /* Increased perspective for larger cube */
  width: auto;
  height: auto;
  z-index: -2;
  pointer-events: none;
  will-change: transform;
}

.cube {
  position: relative;
  transform-style: preserve-3d;
  transition: transform 0.05s ease-out;
  margin: 0 auto;
  will-change: transform;
}

.cube-face {
  position: absolute;
  width: 100%;
  height: 100%;
  background: rgba(255, 255, 255, 0.03);
  border: 1px solid rgba(255, 255, 255, 0.6);
  box-shadow: 0 0 20px rgba(255, 255, 255, 0.15);
  opacity: 0.2;
  backdrop-filter: saturate(180%) blur(4px);
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
  /* sharp edges: no border-radius */
}

.front  { transform: rotateY(0deg) translateZ(var(--half, 200px)); }
.back   { transform: rotateY(180deg) translateZ(var(--half, 200px)); }
.right  { transform: rotateY(90deg) translateZ(var(--half, 200px)); }
.left   { transform: rotateY(-90deg) translateZ(var(--half, 200px)); }
.top    { transform: rotateX(90deg) translateZ(var(--half, 200px)); }
.bottom { transform: rotateX(-90deg) translateZ(var(--half, 200px)); }

.container {
  max-width: 800px;
  height: 100%;
  margin: 0 auto;
  padding: 20px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  position: relative;
  z-index: 1;
}

.header-component {
  width: 100%;
}

.content {
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100%;
  color: #ffffff;
  text-align: center;
  box-sizing: border-box;
}

.main-container {
  width: 100%;
  max-width: 800px;
  height: 100%;
  padding-bottom: 100px;
}

.fade-enter-active, .fade-leave-active {
  transition: opacity 1.2s ease;
}
.fade-enter-from, .fade-leave-to {
  opacity: 0;
}

@media (max-width: 900px) {
  .container {
    max-width: 90%;
  }
}

@media (max-width: 600px) {
  .container {
    max-width: 100%;
    padding: 10px;
  }
  
  .cube-container {
    perspective: calc(1800px + 5vw); /* Adjusted for larger cube */
  }
}

@media (max-width: 480px) {
  .cube-container {
    perspective: 1500px; /* Increased for larger cube */
  }
}

/* Loading Screen Styles */
.loading-screen {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 10;
}

.loading-content {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 30px;
  color: white;
  animation: fadeIn 0.8s ease-out;
  z-index: 20;
}

.loading-name {
  font-size: 42px;
  font-weight: 500;
  margin-bottom: 0.5rem;
}

.loading-title {
  font-size: 18px;
  font-weight: 300;
}

@keyframes fadeIn {
  from { opacity: 0; transform: translateY(10px); }
  to { opacity: 1; transform: translateY(0); }
}

/* Responsive loading screen */
@media (max-width: 600px) {
  .loading-content {
    flex-direction: column;
    gap: 5px;
  }
  
  .loading-name {
    font-size: 24px;
  }
  
  .loading-title {
    font-size: 14px;
  }
}
</style>

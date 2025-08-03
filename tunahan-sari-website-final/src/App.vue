<template>
  <div id="app">
    <!-- Rotating Gradient Background -->
    <div class="gradient-background"></div>
    
    
    <!-- Common Container for Header and Content -->
    <div class="container">
      <!-- Header Component -->
      <AppHeader class="header-component" @section-selected="setActiveSection" />
      
      <!-- Main Content Container -->
      <div class="content">
        <transition name="fade" mode="out-in">
          <component :is="currentComponent" class="main-container" />
      </transition>
      </div>
    </div>
  </div>
</template>

<script>
import AppHeader from './components/AppHeader.vue';
import AppHome from './components/AppHome.vue';
import AppProjects from './components/AppProjects.vue';
import AppSkills from './components/AppSkills.vue';
// Import other components as needed
import AppExperience from './components/AppExperience.vue';
import AppContactFaq from './components/AppContact.vue';

export default {
  name: 'App',
  components: {
    AppHeader,
    AppHome,
    AppProjects,
    AppSkills,
    // Register other components here
    AppExperience,
    AppContactFaq
  },
  data() {
    return {
      activeSection: 'home', // Default section
      // noiseImage is unused since CSS handles the background image
      // You can remove it if not needed
      // noiseImage: require('./assets/random-static.png'), 
    };
  },
  computed: {
    currentComponent() {
      switch(this.activeSection) {
        case 'home':
          return AppHome; // Return component object
        case 'projects':
          return AppProjects; // Return component object
        case 'skills':
          return AppSkills
        // Add cases for other sections as needed
        case 'experience':
          return AppExperience;
        case 'contact':
          return AppContactFaq;
        default:
          return AppHome;
      }
    }
  },
  methods: {
    setActiveSection(section) {
      this.activeSection = section;
    }
  }
};
</script>

<style>
/* Inter Font Import */
@import url('https://fonts.googleapis.com/css2?family=Inter:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&display=swap');

/* Global Styles */
html, body, #app {
  height: 100%;
  margin: 0;
  padding: 0;
  font-family: 'Inter', sans-serif; /* Apply Inter font globally */
}

body {
  overflow: hidden; /* Prevent page scrolling */
}

#app {
  position: relative;
  width: 100%;
  height: 100%;
  overflow: hidden;
  background: transparent; /* Remove default background */
}

/* Rotating Gradient Background */
.gradient-background {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: #111111;
  z-index: -3;
}

/* Rotating Gradient Animation */
@keyframes rotateGradient {
  from {
    transform: translate(-50%, -50%) rotate(0deg);
  }
  to {
    transform: translate(-50%, -50%) rotate(360deg);
  }
}



/* Common Container for Header and Content */
.container {
  max-width: 800px; /* Desired width */
  height: 100%; /* Full height of parent */
  margin: 0 auto; /* Center horizontally */
  padding: 20px; /* Optional padding */
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center; /* Center content vertically */
}

/* Header Styling */
.header-component {
  width: 100%; /* Ensure header takes full width of container */
}

/* Main Content Styling */
.content {
  width: 100%; /* Ensure content takes full width of container */
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
  max-width: 800px; /* Same max-width as the container */
  height: 100%;
  padding-bottom: 100px;
}

/* Vue Logo Styling (Optional) */
.logo {
  width: 150px;
  margin-bottom: 20px;
}

.fade-enter-active, .fade-leave-active {
  transition: opacity 0.3s ease;
}
.fade-enter-from, .fade-leave-to {
  opacity: 0;
}

/* Responsive Design (Optional) */
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
}
</style>

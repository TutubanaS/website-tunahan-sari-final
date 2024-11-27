<template>
  <div>
    <!-- Mobile Header -->
    <header class="header mobile-header">
      <!-- Mobile Burger Menu -->
      <div class="mobile-header-content">
        <div class="name-title-container">
          <div class="name">Tunahan Sarı</div>
          <div class="title">
            Software & Machine Learning<br />
            Engineer
          </div>
        </div>

        <div class="burger-menu" @click="toggleMobileMenu">
          <div :class="['burger-icon', { 'open': isMobileMenuOpen }]">
            <span></span>
            <span></span>
            <span></span>
          </div>
        </div>
      </div>

      <nav :class="['mobile-navigation', { 'open': isMobileMenuOpen }]">
        <ul>
          <li @click="selectSection('home')">
            <a href="#" @click.prevent="setActiveSection('home')">home</a>
          </li>
          <li @click="selectSection('projects')">
            <a href="#" @click.prevent="setActiveSection('projects')">projects</a>
          </li>
          <li @click="selectSection('skills')">
            <a href="#" @click.prevent="setActiveSection('skills')">skills</a>
          </li>
          <li @click="selectSection('experience')">
            <a href="#" @click.prevent="setActiveSection('experience')">experience</a>
          </li>
          <li @click="selectSection('contact')">
            <a href="#" @click.prevent="setActiveSection('contact')">contact & faq</a>
          </li>
        </ul>
      </nav>
    </header>

    <!-- Desktop Header -->
    <header class="header desktop-header">
      <div class="header-content">
        <!-- Title Texts -->
        <div class="name-title-container">
          <div class="name">Tunahan Sarı</div>
          <div class="title">
            Software & Machine Learning<br />
            Engineer
          </div>
        </div>

        <!-- Navigation Menu -->
        <div class="nav-container">
          <div class="header-line"></div>
          <nav class="navigation">
            <ul ref="navList">
              <li :class="{ active: activeSection === 'home' }" ref="homeNavItem">
                <a href="#" @click.prevent="setActiveSection('home')">home</a>
              </li>
              <li :class="{ active: activeSection === 'projects' }" ref="projectsNavItem">
                <a href="#" @click.prevent="setActiveSection('projects')">projects</a>
              </li>
              <li :class="{ active: activeSection === 'skills' }" ref="skillsNavItem">
                <a href="#" @click.prevent="setActiveSection('skills')">skills</a>
              </li>
              <li :class="{ active: activeSection === 'experience' }" ref="experienceNavItem">
                <a href="#" @click.prevent="setActiveSection('experience')">experience</a>
              </li>
              <li :class="{ active: activeSection === 'contact' }" ref="contactNavItem">
                <a href="#" @click.prevent="setActiveSection('contact')">contact & faq</a>
              </li>
              <!-- Indicator -->
              <div class="nav-indicator" :style="indicatorStyle"></div>
            </ul>
          </nav>
        </div>
      </div>
    </header>
  </div>
</template>

<script>
export default {
  name: 'AppHeader',
  data() {
    return {
      activeSection: 'home',
      isMobileMenuOpen: false,
      indicatorStyle: {
        left: '0px',
        width: '0px',
      },
    }
  },
  mounted() {
    this.$nextTick(() => {
      this.updateIndicator();
    });
    window.addEventListener('resize', this.updateIndicator);
  },
  beforeUnmount() {
    window.removeEventListener('resize', this.updateIndicator);
  },
  methods: {
    toggleMobileMenu() {
      this.isMobileMenuOpen = !this.isMobileMenuOpen;
    },
    selectSection(section) {
      this.setActiveSection(section);
      this.isMobileMenuOpen = false; // Close menu after selection
    },
    setActiveSection(section) {
      this.activeSection = section;
      this.$emit('section-selected', section);
      this.$nextTick(() => {
        this.updateIndicator();
      });
    },
    updateIndicator() {
      const activeNavItem = this.$refs[`${this.activeSection}NavItem`];
      if (activeNavItem) {
        const navListRect = this.$refs.navList.getBoundingClientRect();
        const itemRect = activeNavItem.getBoundingClientRect();
        const left = itemRect.left - navListRect.left;
        const width = itemRect.width;
        this.indicatorStyle = {
          left: `${left}px`,
          width: `${width}px`,
        };
      }
    },
  },
}
</script>

<style scoped>
.header {
  width: 100%;
  background-color: transparent;
  color: #fff;
}

/* Mobile Styles */
@media (max-width: 600px) {
  .desktop-header {
    display: none;
  }

  .mobile-header {
    
    margin-top: 25px;
    
  }

  .mobile-header .mobile-header-content {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding-bottom: 20px;
  }

  .mobile-header .name-title-container {
    display: flex;
    flex-direction: column;
    gap: 5px;
  }

  .mobile-header .name {
    font-size: 24px;
    font-weight: 500;
  }

  .mobile-header .title {
    font-size: 14px;
    font-weight: 300;
  }

  /* Burger Menu Styling */
  .mobile-header .burger-menu {
    display: block;
    cursor: pointer;
    z-index: 1000;
    position: relative;
  }

  .mobile-header .burger-icon {
    width: 30px;
    height: 20px;
    position: relative;
    transform: rotate(0deg);
    transition: .5s ease-in-out;
    cursor: pointer;
  }

  .mobile-header .burger-icon span {
    display: block;
    position: absolute;
    height: 3px;
    width: 100%;
    background: #fff;
    border-radius: 9px;
    opacity: 1;
    left: 0;
    transform: rotate(0deg);
    transition: .25s ease-in-out;
  }

  .mobile-header .burger-icon span:nth-child(1) {
    top: 0px;
  }

  .mobile-header .burger-icon span:nth-child(2) {
    top: 10px;
  }

  .mobile-header .burger-icon span:nth-child(3) {
    top: 20px;
  }

  .mobile-header .burger-icon.open span:nth-child(1) {
    top: 10px;
    transform: rotate(135deg);
  }

  .mobile-header .burger-icon.open span:nth-child(2) {
    opacity: 0;
    left: -60px;
  }

  .mobile-header .burger-icon.open span:nth-child(3) {
    top: 10px;
    transform: rotate(-135deg);
  }

  /* Mobile Navigation Styling */
  .mobile-header .mobile-navigation {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(31, 31, 31, 0.95);
    display: flex;
    align-items: center;
    justify-content: center;
    transform: translateX(-100%);
    transition: transform 0.3s ease-in-out;
    z-index: 100;
  }

  .mobile-header .mobile-navigation.open {
    transform: translateX(0);
  }

  .mobile-header .mobile-navigation ul {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 30px;
    list-style: none;
    padding: 0;
    margin: 0;
  }

  .mobile-header .mobile-navigation a {
    color: #fff;
    text-decoration: none;
    font-size: 24px;
    font-weight: 300;
    transition: color 0.3s;
  }

  .mobile-header .mobile-navigation a:hover {
    color: #f0f0f0;
  }
}

/* Desktop Styles */
@media (min-width: 601px) {
  .mobile-header {
    display: none;
  }

  .desktop-header {
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 20px 0;
   
  }

  .desktop-header .header-content {
    width: 100%;
  }

  /* Nav Container Styles */
  .desktop-header .nav-container {
    flex-shrink: 0;
    width: 100%;
    max-width: 800px;
    margin: 0 auto;
  }

  /* Title Texts Styles */
  .desktop-header .name-title-container {
    display: flex;
    justify-content: center;
    align-items: center;
    margin-bottom: 20px;
    gap: 30px;
  }

  .desktop-header .name {
    font-size: 42px;
    font-weight: 500;
  }

  .desktop-header .title {
    font-size: 18px;
    font-weight: 300;
  }

  /* Header Line */
  .desktop-header .header-line {
    width: 100%;
    height: 2px;
    background-color: #fff;
    margin-bottom: 5px;
  }

  /* Navigation Styles */
  .desktop-header .navigation {
    position: relative;
    width: 100%;
  }

  .desktop-header .navigation ul {
    width: 100%;
    list-style: none;
    display: flex;
    justify-content: space-between;
    gap: 50px;
    margin: 0;
    padding: 0;
    position: relative;
  }

  .desktop-header .navigation li {
    position: relative;
  }

  .desktop-header .navigation a {
    color: #fff;
    text-decoration: none;
    font-weight: 300;
    font-size: 18px;
    padding: 5px 10px;
    transition: color 0.3s;
  }

  .desktop-header .navigation a:hover {
    color: #f0f0f0;
    cursor: pointer;
  }

  .desktop-header .nav-indicator {
    position: absolute;
    bottom: 0;
    left: 0;
    height: 100%;
    background-color: rgba(255, 255, 255, 0.8);
    transition: left 0.5s cubic-bezier(0.4, 0, 0.2, 1), width 0.5s cubic-bezier(0.4, 0, 0.2, 1);
    pointer-events: none;
    z-index: -1;
  }

  .desktop-header .navigation li.active a {
    color: #1f1f1f;
  }

}
</style>

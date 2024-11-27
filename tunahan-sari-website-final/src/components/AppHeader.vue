<!-- src/components/Header.vue -->
<template>
    <header class="header">
      <div class="header-content">
        <!-- Başlık Metinleri -->
        <div class="name-title-container">
          <div class="name">Tunahan Sarı</div>
          <div class="title">
            Software & Machine Learning<br />
            Engineer
          </div>
        </div>
        
        <!-- Navigasyon Menüsü -->
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
              <!-- Kaydırıcı Göstergesi -->
              <div class="nav-indicator" :style="indicatorStyle"></div>
            </ul>
          </nav>
        </div>
      </div>
    </header>
  </template>
  
  <script>
  export default {
    name: 'AppHeader',
    data() {
      return {
        activeSection: 'home', // Varsayılan bölüm
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
    beforeUnmount() { // Vue 3 için beforeDestroy yerine beforeUnmount kullanılır
      window.removeEventListener('resize', this.updateIndicator);
    },
    methods: {
      setActiveSection(section) {
        this.activeSection = section;
        this.$nextTick(() => {
          this.updateIndicator();
        });
        this.$emit('section-selected', section); // Add this line
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

  .header-content {
    width: 100%;
  }

  /* Navigasyon Stilleri */
  .navigation {
    position: relative; /* Kaydırıcı göstergesinin konumlandırılması için relative */
    width: 100%;
  }
  
  .navigation ul {
    width: 100%;
    list-style: none;
    display: flex;
    justify-content: space-between; /* Navigasyon öğelerini ortalar */
    gap: 50px; /* Öğeler arasındaki boşluk */
    margin: 0;
    padding: 0;
    position: relative;
  }
  
  .navigation li {
    position: relative;
  }
  
  .navigation a {
    color: #fff;
    text-decoration: none;
    font-weight: 300;
    font-size: 18px;
    padding: 5px 10px; /* Gerektiğinde padding ayarı */
    transition: color 0.3s; /* Metin renginde yumuşak geçiş */
  }
  
  .navigation a:hover {
    color: #f0f0f0; /* Üzerine gelindiğinde renk değişikliği */
    cursor: pointer;
  }
  
  .nav-indicator {
    position: absolute;
    bottom: 0;
    left: 0;
    height: 100%; /* Adjust height to cover the text */
    background-color: rgba(255, 255, 255, 0.8); /* Yarı saydam beyaz arka plan */
    transition: left 0.5s cubic-bezier(0.4, 0, 0.2, 1), width 0.5s cubic-bezier(0.4, 0, 0.2, 1);
    pointer-events: none; /* Etkileşimleri yok say */
    z-index: -1; /* Navigasyon öğelerinin arkasında */
  }
  
  .navigation li.active a {
    color: #1f1f1f; /* Aktif olduğunda metin rengi siyah */
  }
  
  /* Nav Container Stilleri */
  .nav-container {
    flex-shrink: 0;
    width: 100%;
    max-width: 800px;
    margin: 0 auto;
  }
  
  /* Başlık Metinleri Stilleri */
  .name-title-container {
    display: flex;
    justify-content: center; /* Center the content */
    align-items: center;
    margin-bottom: 20px;
    gap: 30px; /* Add some space between the name and title */
  }
  
  .name {
    font-size: 42px; /* Gerektiğinde boyut ayarı */
    font-weight: 500;
  }
  
  .title {
    font-size: 18px; /* Gerektiğinde boyut ayarı */
    font-weight: 300;
  }
  
  /* Header Stili */
  .header {
    width: 100%;
    background-color: transparent;
    color: #fff;
    display: flex;
    flex-direction: column;
    align-items: center; /* İçeriği ortalar */
    padding: 20px 0; /* Dikey padding */
    margin-top: 50px;
  }
  
  /* Header Çizgisi */
  .header-line {
    width: 100%;
    height: 2px;
    background-color: #fff;
    margin-bottom: 5px;
  }
  
  /* Responsive Ayarlamalar */
  @media (max-width: 600px) {
    .name {
      font-size: 24px;
    }
  
    .title {
      font-size: 14px;
    }
  
    .navigation a {
      font-size: 16px;
    }
  
    .nav-indicator {
      height: 1px;
    }
  
    .navigation ul {
      gap: 20px;
    }
  }
  </style>
  
<template>
    <div class="projects">
      <div
        class="rectangle"
        ref="rectangle"
        :class="{ 'centered': !isOverflowing }"
      >
        <!-- Contact Section -->
        <div class="experience-section">
          <div class="experience-item">
            <!-- Added contact-header wrapper -->
            <div class="contact-header">
              <h3 class="experience-title">contacts?</h3>
              <div class="contact-links">
                <a
                  href="mailto:tunahansari11@gmail.com"
                  target="_blank"
                  rel="noopener noreferrer"
                  >email</a
                >
                <a
                  href="https://www.linkedin.com/in/tunahan-sari"
                  target="_blank"
                  rel="noopener noreferrer"
                  >linkedIn</a
                >
                <a
                  href="https://github.com/TutubanaS"
                  target="_blank"
                  rel="noopener noreferrer"
                  >github</a
                >
              </div>
            </div>
            <p class="experience-description">
              need my cv? text me on linkedin for the fastest reply.
            </p>
          </div>
  
          <!-- Other experience items -->
          <div class="experience-item">
            <h3 class="experience-title">
              how can i be a software and machine learning engineer at once?
            </h3>
            <p class="experience-description">
              i studied computer science & engineering and began as a machine
              learning engineer to my career but soon realized i needed to build
              APIs, manage databases, and write robust back-end code for all this
              AI to work. since one depends on the other, i work with both.
            </p>
          </div>
  
          <div class="experience-item">
            <h3 class="experience-title">
              do i freelance for short-term projects?
            </h3>
            <p class="experience-description">
              yes, shoot me an email or text me on linkedin with the project
              details, and i'll see how I can contribute :)
            </p>
          </div>
  
          <div class="experience-item">
            <h3 class="experience-title">
              what is my favourite series on iracing?
            </h3>
            <p class="experience-description">
              formula vee by far. even though it's a rookie series, the higher
              your irating, the better the drivers you match with, and the races
              become hella fun!
            </p>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    name: "AppContact",
    data() {
      return {
        isOverflowing: false,
      };
    },
    methods: {
      /**
       * Checks whether the content inside the `.rectangle` is overflowing.
       * Sets `isOverflowing` to `true` if content exceeds the container's height.
       */
      checkOverflow() {
        this.$nextTick(() => {
          const el = this.$refs.rectangle;
          if (el) {
            this.isOverflowing = el.scrollHeight > el.clientHeight;
          }
        });
      },
    },
    mounted() {
      // Initial check for overflow when component is mounted
      this.checkOverflow();
      // Add event listener to handle window resize
      window.addEventListener("resize", this.checkOverflow);
    },
    beforeUnmount() {
      // Clean up the event listener when component is destroyed
      window.removeEventListener("resize", this.checkOverflow);
    },
  };
  </script>
  
  <style scoped>
  .projects {
    display: flex;
    justify-content: center;
    align-items: center;
    font-family: "Inter", sans-serif;
    box-sizing: border-box;
  }
  
  .rectangle {
    width: 100%;
    height: 60vh;
    max-width: 800px;
    background-color: rgba(51, 51, 51, 0.0);
    color: #ffffff;
    box-sizing: border-box;
    text-align: left;
    font-weight: 300;
    overflow: auto;
  
    /* Scrollbar styles for Webkit browsers */
    &::-webkit-scrollbar {
      width: 8px;
    }
  
    &::-webkit-scrollbar-track {
      background: #444; /* Track color */
      border-radius: 4px;
    }
  
    &::-webkit-scrollbar-thumb {
      background-color: #888; /* Scrollbar thumb color */
      border-radius: 4px;
      border: 2px solid #333; /* Adds space around the thumb */
    }
  
    &::-webkit-scrollbar-thumb:hover {
      background-color: #555; /* Scrollbar thumb color on hover */
    }
  
    /* Scrollbar styles for Firefox */
    scrollbar-width: thin; /* "auto" or "thin" */
    scrollbar-color: #88888800 #44444400; /* thumb and track color */
  }
  
  .centered {
    display: flex;
    flex-direction: column;
    justify-content: center;
  }
  
  .experience-section {
    padding: 0px;
  }
  
  .experience-item {
    margin-bottom: 40px;
  }
  
  .experience-title {
    font-size: 1.2em;
    font-weight: 500;
    margin: 0 0 10px 0;
  }
  
  .experience-description {
    font-size: 1em;
    font-weight: 300;
    margin: 10px 0 0 0;
  }
  
  .contact-links {
    display: flex;
    gap: 15px;
  }
  
  .contact-links a {
    color: #ffffff;
    text-decoration: none;
    font-weight: 500;
    transition: color 0.3s;
  }
  
  .contact-links a:hover {
    color: #dd63ed;
  }

  .contact-header {
    display: flex;
    align-items: center;
    justify-content: space-between;
  }
  
  /* Mobile Styles */
  @media (max-width: 600px) {
    .contact-header {
      display: flex;
      align-items: center;
      justify-content: space-between;
      flex-wrap: wrap;
    }
  
    .contact-header .experience-title {
      margin-bottom: 0;
    }
  
    .contact-links {
      margin-top: 10px;
    }
  
    .experience-description {
      margin-top: 10px;
    }
    
    .experience-section {
    padding: 20px;
  }

  }
  </style>
  
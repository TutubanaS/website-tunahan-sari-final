<template>
    <div class="projects">
      <div
        class="rectangle"
        ref="rectangle"
        :class="{ 'centered': !isOverflowing }"
      > 
        <!-- ArtVista Project Section -->
        <div class="artvista-section">
        <div class="project-header">
            <h2 class="project-title">artvista app; co-founder, tech lead</h2>
            <div class="project-links">
                <a href="https://artvista.app" target="_blank" rel="noopener noreferrer">website</a>
                <a href="https://play.google.com/store/apps/details?id=com.artvista&hl=en" target="_blank" rel="noopener noreferrer">android</a>
                <a href="https://apps.apple.com/nl/app/artvista-art-companion/id6503986684?l=en-GB" target="_blank" rel="noopener noreferrer">ios</a>
            </div>
        </div>
          <p class="project-description">
            visiting a museum, exhibition, or gallery? artvista recognizes the artwork in front of you provides you a guide like experience thanks to its ai. currently the app is available on both ios and android.
          </p>

          <p class="project-description">
           i’m in charge of the back-end, ai systems, and product designs, together with vlad. within artvista, we train and deploy computer vision models and transformers to recognize art pieces. build rag pipelines, and manage the app’s infrastructure.
          </p>

          
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    name: 'AppProjects',
    data() {
      return {
        isOverflowing: false
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
      }
    },
    mounted() {
      // Initial check for overflow when component is mounted
      this.checkOverflow();
      // Add event listener to handle window resize
      window.addEventListener('resize', this.checkOverflow);
    },
    beforeUnmount() {
      // Clean up the event listener when component is destroyed
      window.removeEventListener('resize', this.checkOverflow);
    }
  };
  </script>
  
  <style scoped>

  .projects {
    display: flex;
    justify-content: center;
    align-items: center;
    font-family: 'Inter', sans-serif;
    box-sizing: border-box;
  }
  
  .rectangle {
    width: 100%;
    height: 60%; /* Set the height of the rectangle */
    max-width: 800px; /* Same max-width as the header */
  
    background-color: rgba(51, 51, 51, 0.0); /* Transparent background */
    color: #ffffff; /* White text color for contrast */
  
    box-sizing: border-box; /* Include padding in the element's total width and height */
    text-align: left; /* Align text to the left */
    font-weight: 300; /* Light font weight */
  
    /* Overflow properties */
    overflow: auto; /* Enables scrollbar when content overflows */
  
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
  
  /* Centered class for vertical centering when content does not overflow */
  .centered {
    display: flex;
    flex-direction: column;
    justify-content: center;
  }
  
  .rectangle p {
    margin: 0 0 10px; /* Add some margin between paragraphs */
    line-height: 1.5; /* Improve readability */
    margin-bottom: 30px; /* Adjusted spacing */
  }
  
  /* --- New Styles for Skills Section --- */
  .skills-section {
    margin-top: 40px;
  }

  .tech-stack-container {
    background-color: rgba(255, 255, 255, 0.8); /* Semi-transparent white background */
    padding: 5px 20px; /* Adjust padding as needed */
    border-radius: 0px; /* Rounded corners */
    color: #000; /* Black text */
    margin-bottom: 20px; /* Space below the container */
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1); /* Optional: subtle shadow for depth */
    }
  
  .skills-title {
    font-size: 1.5em;
    font-weight: 500;
    margin-bottom: 10px;
    
  }
  
  .skills-list {
    list-style-type: disc;
    padding-left: 0px;
    
  }
  
  .skills-list li {
    margin-bottom: 5px;
  }
  
  /* --- New Styles for ArtVista Project Section --- */
  .artvista-section {
    margin-top: 20px;
  }

  .project-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding-bottom: 40px;
}
  
  .project-title {
    font-size: 1.75em;
    font-weight: 500;
    margin-bottom: 10px;
    color: #ffffff;
    
  }
  
  .project-description {
    font-size: 1em;
    line-height: 1.6;
    color: #ffffff;
  }
  
  .project-links {
    display: flex;
    gap: 15px;
    margin-top: 15px;
  }
  
  .project-links a {
    color: #ffffff; /* DodgerBlue color for links */
    text-decoration: none;
    font-weight: 500;
    transition: color 0.3s;
  }
  
  .project-links a:hover {
    color: #dd63ed; /* Light blue on hover */
  }
  </style>
  
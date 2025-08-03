<template>
    <div class="skills">
      <div
        class="rectangle"
        ref="rectangle"
        :class="{ 'centered': !isOverflowing }"
      > 
        <!-- ArtVista Project Section -->
        <div class="artvista-section">
          <!-- Skills Section -->
          <div class="tech-stack-container">
            <h3 class="skills-title">tech-stack</h3>
            <ul class="skills-list">
              <p>python, pyTorch, tensorflow, langchain, openai, fastapi, django</p>
              <p>kotlin, java, postgresql, docker, kubernetes, digitalocean, google cloud</p>
              <p>javascript, typescript, vue, figma</p>
            </ul>
          </div>

          <div class="tech-stack-container">
            <h3 class="skills-title">ai-concepts</h3>
            <ul class="skills-list">
                <p>ml; neural nets, transformers, ann</p>
                <p>computer vision; yolo, efficientdet, tflite</p>
                <p>llm; rag, langchain, vectordbs (pgvector)</p>

            </ul>
          </div>



          <div class="tech-stack-container">
            <h3 class="skills-title">languages</h3>
            <ul class="skills-list">
              <p>english - full proficiency, turkish - native, dutch - competing with 3yr-olds</p>
              
            </ul>
          </div>



          



          
          
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
  .skills {
    display: flex;
    justify-content: center;
    align-items: center;
    font-family: 'Inter', sans-serif;
    box-sizing: border-box;
  }
  
  .rectangle {
    width: 100%;
    height: 60vh; /* Set the height of the rectangle */
    max-width: 800px; /* Same max-width as the header */
    background-color: rgba(51, 51, 51, 0.0); /* Transparent background */
    color: #ffffff; /* White text color for contrast */
    box-sizing: border-box; /* Include padding in the element's total width and height */
    text-align: left; /* Align text to the left */
    font-weight: 300; /* Light font weight */
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
  
  .rectangle p {
    margin: 0 0 10px; /* Add some margin between paragraphs */
    line-height: 1.5; /* Improve readability */
    margin-bottom: 30px; /* Adjusted spacing */
  }
  
  .tech-stack-container {
    background-color: transparent; /* No background */
    padding: 5px 20px; /* Adjust padding as needed */
    border-radius: 0px; /* Rounded corners */
    color: #ffffff; /* White text */
    margin-bottom: 20px; /* Space below the container */
    box-shadow: none; /* No shadow */
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
  
  .skills-description {
    font-size: 0.9em;
    line-height: 1.4;
    margin-top: 10px;
    color: #333; /* Description text color */
  }
  
  .additional-text {
    font-size: 1em; /* Adjust font size */
    font-weight: 200; /* Regular font weight */
    margin-bottom: 20px; /* Space below the text */
    color: #ffffff; /* Adjust text color */
    text-align: left; /* Align text to center if desired */
  }

  
  </style>
  
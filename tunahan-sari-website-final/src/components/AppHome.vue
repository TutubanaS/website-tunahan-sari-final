<template>
  <div class="home">
    <div
      class="rectangle"
      ref="rectangle"
      :class="{ 'centered': !isOverflowing }"
    >
      <p>
        born in 2001 in ankara, türkiye, and now based in eindhoven, netherlands. graduated from tu/eindhoven with a bachelor's in computer science.</p>
      <p>
        since i co-founded a company to revolutionize how we engage with art, i realized i needed a website to reflect my design understanding. so, i created this website.</p>
      <p>
        i offer software development and AI/ML services through vista technologies for freelancing projects. check the contact & faq page for more information.</p>
      <p>
        next to software development, i paint and design. the website's design/code belongs to me.</p>
    </div>
  </div>
</template>

<script>
export default {
  name: 'AppHome',
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
.home {
  display: flex;
  justify-content: center;
  align-items: center;
  /* Removed height: 100vh; to allow parent to control height */
  font-family: 'Inter', sans-serif;
  box-sizing: border-box; /* Include padding in the element's total width and height */
  
}

.rectangle {
  width: 100%;
  height: 60vh; /* Set the height of the rectangle */
  max-width: 800px; /* Same max-width as the header */
  
  background-color: rgba(51, 51, 51, 0); /* Dark color */
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
  margin-bottom: 30px;
  /* Removed extra margin-bottom to prevent excessive spacing */
}


</style>

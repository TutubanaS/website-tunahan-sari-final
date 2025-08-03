<template>
  <div class="projects">
    <div
      class="rectangle"
      ref="rectangle"
      :class="{ 'centered': !isOverflowing }"
    >
      <!-- Experience Section -->
      <div class="experience-section">
        <div class="experience-item">
          <div class="experience-header">
            <h3 class="experience-title">2025-</h3>
            <div class="experience-details">
              <p class="experience-role">ai & software engineer | consultant</p>
              <p class="experience-company">@ accenture</p>
            </div>
          </div>
        </div>
        <div class="experience-item">
          <div class="experience-header">
            <h3 class="experience-title">2023-2025</h3>
            <div class="experience-details">
              <p class="experience-role">ai & software engineer</p>
              <p class="experience-company">@ enter</p>
            </div>
          </div>
        </div>
        <div class="experience-item">
          <div class="experience-header">
            <h3 class="experience-title">2022-2023</h3>
            <div class="experience-details">
              <p class="experience-role">machine learning engineer</p>
              <p class="experience-company">@ universaldot foundation</p>
            </div>
          </div>
        </div>
        <div class="experience-item">
          <div class="experience-header">
            <h3 class="experience-title">2021-2023</h3>
            <div class="experience-details">
              <p class="experience-role">tutoring assistant</p>
              <p class="experience-company">@ tu eindhoven</p>
            </div>
          </div>
        </div>
        <p class="experience-note">
          for more details, check and request my cv through linkedin.<br />
          check my linkedIn at contact & faq.
        </p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "AppExperience",
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
  height: 60%;
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
  padding: 20px;
}

.experience-item {
  margin-bottom: 40px;
}

.experience-header {
  display: flex;
  flex-direction: row-reverse; /* Keep year on the right for desktop */
  justify-content: space-between;
  align-items: center;
}

.experience-title {
  font-size: 1.2em;
  font-weight: 300;
  margin: 0;
}

.experience-details {
  display: flex;
  flex-direction: column;
}

.experience-role,
.experience-company {
  margin: 0;
  font-size: 1.2em;
  font-weight: 300;
}

.experience-note {
  font-size: 1em;
  font-weight: 200;
  margin-top: 20px;
}

/* Mobile styles */
@media (max-width: 600px) {
  .experience-header {
    flex-direction: column;
    align-items: flex-start;
  }

  .experience-title {
    font-size: 1.2em;
    font-weight: 500; /* Year bold */
    margin-bottom: 5px;
  }

  .experience-role {
    font-size: 1em;
    font-weight: 400;
  }

  .experience-company {
    font-size: 1em;
    font-weight: 300;
  }

  
}
</style>

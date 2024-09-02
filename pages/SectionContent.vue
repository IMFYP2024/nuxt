<template>
  <div>
    <div class="image-container">
      <img :src="imageSrc" alt="Image" />
    </div>
    <div class="text-container">
      <p v-html="description"></p>
      <div class="dropdown-list" v-show="showDropdown">
        <transition-group name="list" tag="div">
          <div
            v-for="(item, index) in items"
            :key="index"
            class="dropdown-item"
            :class="{ 'active-blue': activeTab === item.link }"
            :style="{ animationDelay: `${index * 0.5}s`, backgroundColor: item.color }"
            @click="$emit('change-tab', item.link)"
          >
            {{ item.text }}
          </div>
        </transition-group>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'SectionContent',
  props: {
    imageSrc: String,
    description: String,
    items: Array,
    activeTab: String,
  },
  data() {
    return {
      showDropdown: true,
    };
  }
};
</script>

<style scoped>
.image-container {
  width: 100%;
  text-align: center;
  margin-bottom: 1rem;
}

.image-container img {
  border-radius: 10%/50%;
  width: 100%;
  height: auto;
  max-width: 400px;
}

.text-container {
  text-align: center;
  margin-bottom: 2rem;
  width: 100%;
  position: relative;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.text-container p {
  font-size: 1.2rem;
  margin-top: 1rem;
  padding: 0 1rem;
  text-align: center;
  transition: background-color 0.3s;
}

.dropdown-list {
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.dropdown-item {
  width: 90%;
  max-width: 400px;
  text-align: center;
  margin-top: 1rem;
  padding: 0.5rem 1rem;
  font-size: 1.2rem;
  border-radius: 5px;
  transition: background-color 0.3s, transform 0.3s;
  cursor: pointer;
  display: flex;
  justify-content: center;
  align-items: center;
  white-space: nowrap;
}

.dropdown-item:hover {
  background-color: rgba(195, 198, 221, 1);
}

.active-blue {
  color: white !important;
}
</style>

<template>
  <div class="background1">
    <!-- Title -->
    <h1>樓層介紹</h1>

    <!-- Floor Selection Dropdown -->
    <div class="select-wrapper">
      <select v-model="selectedFloor" class="floor-select">
        <option v-for="floor in floors" :key="floor" :value="floor">
          {{ floor }}
        </option>
      </select>
    </div>

    <div class="container">
      <!-- Outer Square -->
      <div class="outer-square">
        <!-- Grid Layout Area -->
        <div class="grid-container">
          <div v-for="(image, index) in images" :key="index" :class="'item' + (index + 1)">
            <img :src="image" alt="Grid Image" />
          </div>
        </div>
        <!-- Inner Square -->
        <div class="inner-square">
          <p>會議廳<br>7212<br>2樓</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, onMounted } from 'vue';
import { useRoute } from 'vue-router';

export default {
  setup() {
    const floors = ['1F', '2F', '3F', '4F', '5F', '6F', '7F', '8F', '9F'];
    const selectedFloor = ref('1F');
    const route = useRoute();

    // Update selectedFloor based on route query
    onMounted(() => {
      if (route.query.floor && floors.includes(route.query.floor)) {
        selectedFloor.value = route.query.floor;
      }
    });

    const images = [
      "/Images/zhongshang/toilet.JPEG",
      "/Images/zhongshang/2f.JPEG",
      "/Images/zhongshang/2f1.JPEG",
      "/Images/zhongshang/2f2.JPEG",
      "/Images/zhongshang/female.jpg",
      "/Images/zhongshang/2f.4.JPEG",
      "/Images/zhongshang/2f_2.JPEG",
      "/Images/zhongshang/2f.4.1.JPEG",
      "/Images/zhongshang/2f.4.2.JPEG",
      "/Images/zhongshang/lift.1.jpg",
      "/Images/zhongshang/2f.3.1.JPEG",
      "/Images/zhongshang/2f.3.2.JPEG",
      "/Images/zhongshang/2f.3.3.JPEG",
      "/Images/zhongshang/lift.2.jpg",
    ];

    return {
      selectedFloor,
      floors,
      images
    };
  }
};
</script>

<style scoped>
body {
  margin: 0;
}

.body{
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center; /* 中心對齊 */
  box-sizing: border-box;
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  background-image: url('/Images/bg.jpg');
}

.background1 h1 {
  margin-bottom: 10px;
  font-size: 24px;
  color: white;
  text-shadow: 1px 1px 4px rgba(0, 0, 0, 0.5);
}

/* 選擇器包裹容器，垂直置中 */
.select-wrapper {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-bottom: 20px;
}

.floor-select {
  padding: 10px;
  font-size: 16px;
  border-radius: 5px;
  border: 1px solid #ddd;
  background-color: white;
}

.container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 60vh;
}

.outer-square {
  position: relative;
  width: 400px;  /* 外層正方形大小 */
  height: 400px;
  background-color: #f0f0f0;
}

.inner-square {
  position: absolute;
  width: 180px;  /* 內層正方形大小 */
  height: 50%;
  background-color: #ffffff;
  top: 60%;
  left: 50%;
  transform: translate(-50%, -50%); /* 內層正方形居中 */
  display: flex;
  align-content: center;
  justify-content: center;
  font-size: 36px;
}

.grid-container {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%; /* 固定寬度 */
  height: 100%; /* 固定高度 */
  display: grid;
  grid-template-columns: repeat(5, 1fr); /* 5列，等分 */
  grid-template-rows: repeat(5, 1fr); /* 5行，等高 */
  gap: 2px;
  z-index: 0;
}

.grid-container > div {
  width: 100%;
  height: 100%;
}

.grid-container img {
  width: 100%;
  height: 100%;
  object-fit: cover; /* 確保圖片填充單元格，保持比例 */
}


/* 可選：為每個網格項定義不同的樣式 */

.item7 {
  background-color: #4dd2ff;
  grid-column: 5;
  grid-row: 2 / 5;
}
.item8 { background-color: #33ccff; grid-row: 3; }
.item9 { background-color: #1ac6ff; grid-row: 4; }
.item10 { background-color: #00bfff; grid-row: 5; }
.item11 { background-color: #00b3e6; grid-row: 5; }
.item12 { background-color: #0099cc; grid-row: 5; }
.item13 { background-color: #0086b3; grid-row: 5; }
.item14 { background-color: #007399; grid-row: 5; }

@media (max-width: 600px) {
  h1 {
    font-size: 20px;
  }

  .background1 {
    padding: 10px;
  }

  .floor-select {
    font-size: 14px;
    padding: 8px;
  }
}
</style>

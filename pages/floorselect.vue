<template>
  <div class="building-container">
    <h1>資訊舘</h1>
    <!-- 建築物頂部的圖標 -->
    <div class="icon-trigger">
      <Icon name="icon-park-solid:click" style="color:black" />
    </div>

    <!-- 建築物和柱子容器 -->
    <div class="building-and-pillar">
      <div class="building">
        <div v-for="floor in floors" :key="floor" class="floor" @click="navigateToBuilding(floor)">
          <span>{{ floor }}</span>
        </div>
      </div>

      <div class="pillar" @click="showModal = true">
        <div class="pillar-text">資訊舘</div>
      </div>
    </div>

    <Modal
      :title="modalTitle"
      :showModal="showModal"
      @closing="closeModal"
    >
      <template #itemText>
        <p>你可以試著點擊 <Icon name="bi:1-circle" /> 來查看一些樓層的區域信息</p>
      </template>
    </Modal>
  </div>
</template>


<script setup>
import { ref } from 'vue'
import { useRouter } from 'vue-router';
const floors = ['1F', '2F', '3F', '4F', '5F', '6F', '7F', '8F', '9F'];
const modalTitle = ref('資訊舘的建成故事');
const showModal = ref(false); 
const router = useRouter();
const navigateToBuilding = (floor) => {
  router.push({ path: 'building', query: { floor } });
};
const closeModal = () => {
  showModal.value = false;
};
</script>

<style scoped>
.building-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  height: 90vh; /* 減去導航欄的高度 */
  padding-bottom: 5%; /* 使用百分比來控制間距 */
  background-color: #f0f0f0;
  position: relative;
}

h1 {
  margin-top: 5px; /* 調整標題距離容器頂部的距離 */
  font-size: 36px; /* 設置標題的字體大小 */
  color: black; /* 設置字體顏色 */
  text-align: center; /* 標題居中對齊 */
  margin-bottom: 20px; /* 設置底部距離 */
}

.icon-trigger {
  position: absolute;
  top: 300px; /* 位置稍微向上調整 */
  left: 76%;
  transform: translateX(-50%);
  cursor: pointer;
}

.building {
  display: flex;
  flex-direction: column-reverse;
  width: 200px;
  border: 2px solid black;
  background-color: #ccc;
}

.floor {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 80px;
  border-top: 1px solid black;
  background-color: white;
  font-weight: bold;
}

.pillar {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 40px;
  height: 550px;
  margin-left: 0; /* 移除左側邊距 */
  background-color: #f9efd0;
  border: 2px solid black;
}

.pillar-text {
  writing-mode: vertical-rl; /* 文字豎立顯示 */
  text-orientation: upright; /* 保持文字方向 */
  font-size: 60px;
  color: #333;
  font-weight: bold;
}

/* 將 building 和 pillar 緊密排列 */
.building-and-pillar {
  display: flex;
  align-items: flex-end; /* 確保它們在垂直方向對齊 */
  height: 70vh;
}

/* 自適應樣式 */
@media screen and (min-width: 620px) {
  .h1{
    font-size: 36px;
  }
  .building {
    width: 100px; /* 縮小建築物的寬度以適應小螢幕 */
  }
  .building-container{
    height: 80vh;
  }
  .floor {
    height: 50px; /* 調整樓層高度以適應更小的螢幕 */
    font-size: 14px; /* 確保文字大小適中 */
  }

  .pillar {
    width: 70px; /* 縮小柱子的寬度 */
    height: 600px; /* 縮小柱子的高度 */
  }

  .pillar-text {
    font-size: 30px; /* 縮小文字大小以適應小螢幕 */
  }

  .icon-trigger {
    font-size: 36px; /* 縮小圖標以適應小螢幕 */
  }
}
@media (min-width: 361px) {
  .building {
    width: 110px;
    height: 60vh;
  }
  .building-container{
    height: 80vh;
  }
  .floor {
    height: 60px;
  }

  .pillar {
    width: 80px;
    height: 550px;
  }

  .pillar-text {
    font-size: 35px;
  }

  .icon-trigger {
    font-size: 48px; /* 小螢幕上的圖標稍微縮小 */
  }
} 

</style>

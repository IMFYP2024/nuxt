<template>
  <div class="building-container">
    <!-- 建築物和柱子容器 -->
    <div class="building-and-pillar">
      <div class="pillar" @click="showModal = true">
        <div class="pillar-text">中商</div>
      </div>
      <div class="building">
        <div v-for="floor in floors" :key="floor" class="floor" @click="navigateToBuilding(floor)">
          <span>{{ floor }}</span>
        </div>
      </div>

      <div class="pillar" @click="showModal = true">
        <div class="pillar-text">大樓</div>
      </div>
    </div>

    <Modal
      :title="modalTitle"
      :showModal="showModal"
      @closing="closeModal"
    >
      <template #itemText>
        <img src="/Images/building.jpeg" alt="資訊舘圖片">
        <p><b>1987年</b>新建「資訊館」破土典禮，由於它是與行政大樓緊挨的一起，所以他的入口比較特殊，是需要從行政大樓進入的</p>
      </template>
    </Modal>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import { useRouter } from 'vue-router';
const floors = ['1F', '2F', '3F', '4F', '5F', '6F', '7F', '8F', '9F'];
const modalTitle = ref('中商大樓的建成故事');
const showModal = ref(false);
const router = useRouter();

const navigateToBuilding = (floor) => {
  router.push({ path: 'zhongshang', query: { floor } });
};

const closeModal = () => {
  showModal.value = false;
};
</script>

<style scoped>
.building-container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  padding: 0;
  margin: 0;
  overflow: hidden; /* 確保在垂直滾動時不會溢出 */
}

.building-and-pillar {
  display: flex;
  align-items: flex-end;
  height: 60%;
}

.building {
  display: flex;
  flex-direction: column-reverse;
  width: 150px;
  border: 2px solid black;
  background-color: #ccc;
  overflow-y: auto; /* 允許垂直滾動 */
}

.floor {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 44px; /* 確保每個樓層有足夠的點擊區域 */
  border-top: 1px solid black;
  background-color: white;
  font-weight: bold;
  font-size: 16px; /* 調整字體大小以保證可讀性 */
}

.pillar {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 50px; /* 增加柱子的寬度，便於點擊 */
  height: 550px;
  margin-left: 0;
  background-color: #a5a39e;
  border: 2px solid black;
  cursor: pointer; /* 增加點擊效果 */
}

.pillar-text {
  writing-mode: vertical-rl;
  text-orientation: upright;
  font-size: 50px;
  color: #333;
  font-weight: bold;
}

@media screen and (max-width: 620px) {
  .building {
    width: 120px;
  }

  .floor {
    height: 44px;
    font-size: 14px; /* 小螢幕上的字體稍微縮小 */
  }

  .pillar {
    width: 90px;
    height: 500px;
  }

  .pillar-text {
    font-size: 35px;
  }
}

@media screen and (max-width: 360px) {
  .building {
    width: 100px;
  }

  .floor {
    height: 40px;
    font-size: 12px; /* 更小螢幕上的字體大小 */
  }

  .pillar {
    width: 30px;
    height: 480px;
  }

  .pillar-text {
    font-size: 30px;
  }
}
</style>

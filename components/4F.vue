<template>
  
  <div class="floor-plan" @click="handleOutsideClick">
    <a href="/other-page" class="link-to-other-page">前往資管系介紹</a>
    <img src="/images/4F.JPG" alt="Floor Plan" class="floor-plan-image" />
    <div
      v-for="area in areas"
      :key="area.id"
      class="clickable-area"
      :style="{ top: area.top + 'px', left: area.left + 'px', width: area.width + 'px', height: area.height + 'px' }"
      @click.stop="openModal(area)"
    >
      <Icon :name="area.icon" class="icon" />
    </div>
    <transition name="fade">
      <Modal
        v-if="showed"
        :title="selectedArea?.name || 'Info'"
        :showModal="showed"
        @closing="closeModal"
      >
        <template v-slot:itemText>
          <!-- 插入圖片 -->
          <img v-if="selectedArea?.image" :src="selectedArea.image" alt="Area Image" class="modal-image" />
          <!-- 其他內容 -->
          
          <p>{{ selectedArea?.description || 'No description available.' }}</p>
          <p v-if="selectedArea?.extraInfo">{{ selectedArea.extraInfo }}</p>
        </template>
      </Modal>
    </transition>
  </div>
</template>

<script setup>
import Modal from '@/components/Modal.vue';
import { ref } from 'vue';

const areas = [
  { 
    id: 1, 
    top: 140, 
    left: 220, 
    width: 30, 
    height: 30, 
    icon: 'bi:1-circle', 
    name: '休閑區', 
    description: '大家都很愛去那邊吃飯', 
    image: '/images/iCloud/4F1.JPEG',
    extraInfo: '還是要看有沒有開風扇'
  },
  { 
    id: 2, 
    top: 155, 
    left: 280, 
    width: 30, 
    height: 30, 
    icon: 'bi:2-circle', 
    name: '資管系辦公室', 
    description: '老師都很親切',
    image:"/images/iCloud/4F3.JPEG",
    extraInfo: '有什麽問題都可以去問'
  },
  { 
    id: 3, 
    top: 95, 
    left: 200, 
    width: 30, 
    height: 30, 
    icon: 'bi:3-circle', 
    name: '資管系教室', 
    description: '電腦設備很好，同學有時會偷打遊戲',
    image:"/images/iCloud/4F2.JPEG",
    extraInfo: '但是是禁止的哦'
  }
];

const selectedArea = ref(null);
const showed = ref(false);

const openModal = (area) => {
  selectedArea.value = area;
  showed.value = true;
};

const closeModal = () => {
  showed.value = false;
};

const handleOutsideClick = (event) => {
  if (showed.value && !event.target.closest('.modal-content')) {
    closeModal();
  }
};
</script>

<style scoped>
.floor-plan {
  position: relative;
  width: 100%;
  overflow: hidden;
  touch-action: none;
}

.floor-plan-image {
  width: 394px;
  height: 241px;
}

.clickable-area {
  position: absolute;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
}

.icon {
  font-size: 24px;
}

.modal-image {
  width: 100%;
  height: auto;
  max-width: 600px; /* Adjust as needed */
  max-height: 400px; /* Adjust as needed */
}

/* Fade transition for modal */
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s;
}

.fade-enter, 
.fade-leave-to /* .fade-leave-active in <2.1.8 */ {
  opacity: 0;
}
.link-to-other-page {
  display: inline-block;
  padding: 8px 12px;
  font-size: 14px;
  border-radius: 5px;
  background-color: green; /* 更換為柔和的藍色 */
  color: white;
  text-decoration: none;
  text-align: center;
  transition: background-color 0.3s ease;
}

.link-to-other-page:hover {
  background-color: #0056b3; /* 更深的藍色 */
}

@media (max-width: 600px) {
  .floor-plan-image {
    max-width: 100%; /* 確保圖片在小屏幕上佔據最大寬度 */
    max-height: 300px; /* 可根據需要調整 */
  }
}

@media (min-width: 601px) and (max-width: 1024px) {
  .floor-plan-image {
    max-width: 100%;
    max-height: 400px; /* 可根據需要調整 */
  }
}

@media (min-width: 1025px) {
  .floor-plan-image {
    max-width: 100%;
    max-height: 600px; /* 可根據需要調整 */
  }
}
</style>

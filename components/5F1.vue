<template>
  
  <div class="floor-plan" @click="handleOutsideClick">
    
    <img src="/Images/iCloud/5F.JPEG" alt="Floor Plan" class="floor-plan-image" />
    <div
  v-for="area in areas"
  :key="area.id"
  class="clickable-area"
  :style="{ 
    top: (area.top / 241) * 100 + '%', 
    left: (area.left / 394) * 100 + '%', 
    width: area.width + 'px', 
    height: area.height + 'px' 
  }"
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
    top: 80, 
    left: 140, 
    width: 30, 
    height: 30, 
    icon: 'mdi:number-1-circle', 
    name: '休閑區', 
    description: '聽説資工系都很愛去吃飯，也看到蠻多人在那邊躺的', 
    image: '/Images/iCloud/5F2.JPEG',
    extraInfo: ''
  },
  { 
    id: 2, 
    top: 85, 
    left: 60, 
    width: 30, 
    height: 30, 
    icon: 'mdi:number-2-circle', 
    name: '資工系教室', 
    description: '少數會有平板的教室，雖然我沒到那邊上過課',
    image:"/Images/iCloud/5F3.JPEG",
    extraInfo: '但每次都看起來很chill的上課氛圍'
  },
  { 
    id: 3, 
    top: 100, 
    left: 190, 
    width: 30, 
    height: 30, 
    icon: 'mdi:number-3-circle', 
    name: '資工系教室', 
    description: '電腦設備很好，同學有時會偷打遊戲',
    image:"/Images/iCloud/5F1.JPEG",
    extraInfo: '但是是禁止的哦'
  },
  { 
    id: 4, 
    top: 140, 
    left: 295, 
    width: 30, 
    height: 30, 
    icon: 'mdi:number-4-circle', 
    name: '資工系辦公室', 
    description: '老師都很親切',
    image:"/Images/iCloud/5F0.JPEG",
    extraInfo: '資工系有什麽問題都可以去問'
  },
  { 
    id: 6, 
    top: 85, 
    left: 40, 
    width: 30, 
    height: 30, 
    icon: 'icomoon-free:arrow-left2', 
    name: '資工系教室', 
    description: '少數會有平板的教室，雖然我沒到那邊上過課',
    image:"/Images/iCloud/5F3.JPEG",
    extraInfo: '但每次都看起來很chill的上課氛圍'
  },
  { 
    id: 7, 
    top: 100, 
    left: 210, 
    width: 30, 
    height: 30, 
    icon: 'icomoon-free:arrow-right2', 
    name: '資工系教室', 
    description: '電腦設備很好，同學有時會偷打遊戲',
    image:"/Images/iCloud/5F1.JPEG",
    extraInfo: '但是是禁止的哦'
  },
  { 
    id: 8, 
    top: 140, 
    left: 315, 
    width: 30, 
    height: 30, 
    icon: 'icomoon-free:arrow-right2', 
    name: '資工系辦公室', 
    description: '老師都很親切',
    image:"/Images/iCloud/5F0.JPEG",
    extraInfo: '資工系有什麽問題都可以去問'
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

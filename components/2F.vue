<template>
  
    <div class="floor-plan" @click="handleOutsideClick">
      <img src="/Images/iCloud/2.JPEG" alt="Floor Plan" class="floor-plan-image" />
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
      top: 125, 
      left: 290, 
      width: 25, 
      height: 25, 
      icon: 'bi:1-circle', 
      name: '校務資訊組', 
      description: '',
      image:"/Images/iCloud/2F4.JPEG",
      extraInfo: ''
    },
    { 
      id: 2, 
      top: 130, 
      left: 220, 
      width: 25, 
      height: 25, 
      icon: 'bi:2-circle', 
      name: '第二會議室', 
      description: '老師們開會的地方',
      image:"/Images/iCloud/2F3.JPEG",
      extraInfo: ''
    },
    { 
      id: 3, 
      top: 110, 
      left: 185, 
      width: 25, 
      height: 25, 
      icon: 'bi:3-circle', 
      name: '第三會議室', 
      description: '歷届的實習媒合會都在這裏',
      image:"/Images/iCloud/2F2.JPEG",
      extraInfo: ''
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
    width: 100%;
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
  
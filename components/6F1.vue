<template>
    <div class="floor-plan" @click="handleOutsideClick">
      <img src="/Images/iCloud/6F2.JPEG" alt="Floor Plan" class="floor-plan-image" />
      <div
        v-for="area in areas"
        :key="area.id"
        class="clickable-area"
        :style="{ top: area.top + 'px', left: area.left + 'px', width: area.width + 'px', height: area.height + 'px' }"
        @click.stop="openModal(area)"
      >
        <Icon :name="area.icon" :style="{ color: area.color }" class="icon" />
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
      id: 2, 
      top: 90, 
      left: 40, 
      width: 30, 
      height: 30, 
      icon: 'icomoon-free:arrow-right2', 
      name: '專題研究室', 
      description: '專題生都一定會待的地方',
      image:"/Images/iCloud/6F5.JPEG",
      extraInfo: '聽説會有柴犬出沒'
    },
    { 
      id: 3, 
      top: 110, 
      left: 320, 
      width: 30, 
      height: 30, 
      icon: 'icomoon-free:arrow-left2', 
      name: '電腦教室和專題研究室', 
      description: '前端是學生上課的',
      image:"/Images/iCloud/6F4.JPEG",
      extraInfo: '但後方也是專題生的研究室'
    },
    { 
      id: 4, 
      top: 90, 
      left: 195, 
      width: 30, 
      height: 30, 
      icon: 'mdi:number-1-circle', 
      color: 'black',
      name: '休閑區', 
      description: '大家都很愛去那邊吃飯', 
      image: '/Images/iCloud/6F3.JPEG',
      extraInfo: '順便討論專題'
    },
    { 
      id: 5, 
      top: 90, 
      left: 65, 
      width: 30, 
      height: 30,  
      icon: 'mdi:number-2-circle', 
      name: '專題研究室', 
      description: '專題生都一定會待的地方',
      image:"/Images/iCloud/6F5.JPEG",
      extraInfo: '聽説會有柴犬出沒'
    },
    { 
      id: 6, 
      top: 110, 
      left: 300, 
      width: 30, 
      height: 30, 
      icon: 'mdi:number-3-circle', 
      name: '電腦教室和專題研究室', 
      description: '前端是學生上課的',
      image:"/Images/iCloud/6F4.JPEG",
      extraInfo: '但後方也是專題生的研究室'
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
    height: 100%;
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
    color: black;
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
  
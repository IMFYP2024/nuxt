<template>
  
    <div class="floor-plan" @click="handleOutsideClick">
      <img src="/Images/iCloud/3F.JPEG" alt="Floor Plan" class="floor-plan-image" />
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
      left: 70, 
      width: 25, 
      height: 25, 
      icon: 'bi:1-circle', 
      name: '電腦教室', 
      description: '學生上課的地方',
      image:"/Images/iCloud/3F1.JPEG",
      extraInfo: ''
    },
    { 
      id: 2, 
      top: 80, 
      left: 335, 
      width: 25, 
      height: 25, 
      icon: 'bi:2-circle', 
      name: '教學資訊組', 
      description: '負責電腦教室教室管理與使用，數位學習平臺',
      image:"/Images/iCloud/3F2.JPEG",
      extraInfo: '全校授權軟體與相關作業'
    },
    { 
      id: 3, 
      top: 140, 
      left: 335, 
      width: 25, 
      height: 25, 
      icon: 'bi:3-circle', 
      name: '電腦教室', 
      description: '學生上課的地方',
      image:"/Images/iCloud/3F3.JPEG",
      extraInfo: ''
    },
    {id: 4, 
        top: 140, 
      left: 50, 
      width: 25, 
      height: 25,    
      icon: 'icomoon-free:arrow-left2', 
      name: '電腦教室', 
      description: '學生上課的地方',
      image:"/Images/iCloud/3F1.JPEG",
      extraInfo: ''
    },
    {id: 5, 
        top: 140, 
      left: 355, 
      width: 25, 
      height: 25, 
      icon: 'icomoon-free:arrow-right2', 
      name: '電腦教室', 
      description: '學生上課的地方',
      image:"/Images/iCloud/3F3.JPEG",
      extraInfo: ''
    },
    {id: 6, 
        top: 80, 
      left: 355, 
      width: 25, 
      height: 25,   
      icon: 'icomoon-free:arrow-right2', 
      name: '教學資訊組', 
      description: '負責電腦教室教室管理與使用，數位學習平臺',
      image:"/Images/iCloud/3F2.JPEG",
      extraInfo: '全校授權軟體與相關作業'
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
  
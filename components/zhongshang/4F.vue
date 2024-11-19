<template>
    <div class="background1">
      
      <!-- 影片播放區域 -->
      <div class="video-container">
        <video autoplay muted loop playsinline class="video-content">
          <source :src="videoSource" type="video/mp4" />
          您的瀏覽器不支援影片播放。
        </video>
      </div>
  
      <!-- 點擊區域 -->
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
  
      <!-- 模態框 -->
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
            <p>{{ selectedArea?.description || 'No description available.' }}</p>
            <p v-if="selectedArea?.extraInfo">{{ selectedArea.extraInfo }}</p>
          </template>
        </Modal>
      </transition>
    </div>
  </template>
  
  <script>
  import { ref } from 'vue';
  
  export default {
    setup() {
      const videoSource = ref('https://pub-47f454dfd8af45a19b1d09b9dda1ef29.r2.dev/4floor.mp4'); // 預設樓層影片
      const selectedArea = ref(null);
      const showed = ref(false);
  
      const areas = [
    { 
        id: 1, 
        top: 105, 
        left: 130, 
        width: 20, 
        height: 20, 
        icon: 'bi:1-circle', 
        name: '普通教室', 
        description: '普通學生在普通的一天在普通的教室上課',
        image:"/Images/zhongshang/4f/4f1.JPEG",
        extraInfo: ''
    },
    { 
        id: 2, 
        top: 100, 
        left: 305, 
        width: 20, 
        height: 20, 
        icon: 'bi:3-circle', 
        name: '休閑區', 
        description: '可以在該地方吃飯和休息的地方',
        image:"/Images/zhongshang/4f/4f3.JPEG",
        extraInfo: ''
    },
    { 
        id: 3, 
        top: 80, 
        left: 205, 
        width: 20, 
        height: 20,
        icon: 'bi:2-circle', 
        name: '休閑區', 
        description: '有一個通天的涼亭休息時間都會有很多同學在那邊',
        image:"/Images/zhongshang/4f/4f2.JPEG",
        extraInfo: ''
    },
    { 
        id: 4, 
        top: 70, 
        left: 330, 
        width: 20, 
        height: 20, 
        icon: 'bi:4-circle', 
        name: '會資系系辦', 
        description: '老師很親切，會資系上的任何問題都可以到此詢問',
        image:"/Images/zhongshang/4f/4f4.JPEG",
        extraInfo: ''
    },
    { 
        id: 5, 
        top: 70, 
        left: 50, 
        width: 20, 
        height: 20, 
        icon: 'bi:5-circle', 
        name: '保金服務中心', 
        description: '',
        image:"/Images/zhongshang/4f/4f5.JPEG",
        extraInfo: ''
    }
    
  
];
  
      const openModal = (area) => {
        selectedArea.value = area;
        showed.value = true;
      };
  
      const closeModal = () => {
        showed.value = false;
      };
  
     
  
      return {
        videoSource,
        areas,
        selectedArea,
        showed,
        openModal,
        closeModal,
      };
    }
  };
  </script>
  
  <style scoped>

  
  .video-container {
    width: 100%;
    height: 70%;
    display: flex;
    justify-content: center;
  }
  
  .video-content {
    width: 100%;
    height: 100%;
    object-fit: cover;
    border-radius: 10px;
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
    max-width: 600px;
    max-height: 400px;
  }
  
  .fade-enter-active,
  .fade-leave-active {
    transition: opacity 0.3s;
  }
  
  .fade-enter, 
  .fade-leave-to {
    opacity: 0;
  }
  </style>
  
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
      const videoSource = ref('https://pub-47f454dfd8af45a19b1d09b9dda1ef29.r2.dev/2floor.mp4'); // 預設樓層影片
      const selectedArea = ref(null);
      const showed = ref(false);
  
    const areas = [
    { 
        id: 1, 
        top: 110, 
        left: 100, 
        width: 25, 
        height: 25, 
        icon: 'bi:1-circle', 
        name: '商學院辦公室', 
        description: '以培養培養「商學為本，科技為用」的跨域創新專業管理人才為發展目標',
        image:"/Images/zhongshang/2f/2f3.JPEG",
        extraInfo: ''
    },
    { 
        id: 4, 
        top: 110, 
        left: 135, 
        width: 25, 
        height: 25, 
        icon: 'bi:2-circle', 
        name: '商學院院長室', 
        description: '顧名思義就是商學院的老大在的地方',
        image:"/Images/zhongshang/2f/2f4.JPEG",
        extraInfo: ''
    },
    { 
        id: 2, 
        top: 110, 
        left: 205, 
        width: 25, 
        height: 25, 
        icon: 'bi:3-circle', 
        name: '討論室(大)', 
        description: '可供多位學生一起討論的地方',
        image:"/Images/zhongshang/2f/2f1.JPEG",
        extraInfo: ''
    },
    { 
        id: 3, 
        top: 70, 
        left: 330, 
        width: 25, 
        height: 25, 
        icon: 'bi:4-circle', 
        name: '休閑區', 
        description: '吃飯聊天，吹牛的地方',
        image:"/Images/zhongshang/2f/2f6.JPEG",
        extraInfo: ''
    },
    
    { 
        id: 5, 
        top: 40, 
        left: 210, 
        width: 25, 
        height: 25, 
        icon: 'bi:5-circle', 
        name: '討論室(大)', 
        description: '可供多位學生一起討論的地方',
        image:"/Images/zhongshang/2f/2f5.JPEG",
        extraInfo: ''
    },
    
  
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
  
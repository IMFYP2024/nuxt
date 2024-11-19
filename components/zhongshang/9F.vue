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
    const videoSource = ref('https://pub-47f454dfd8af45a19b1d09b9dda1ef29.r2.dev/9floor.mp4'); // 預設樓層影片
    const selectedArea = ref(null);
    const showed = ref(false);

    const areas = [
  { 
      id: 1, 
      top: 105, 
      left: 120, 
      width: 20, 
      height: 20, 
      icon: 'bi:1-circle', 
      name: '租稅管理與理財規劃專案研究室', 
      description: '',
      image:"/Images/zhongshang/9f/9f1.JPEG",
      extraInfo: ''
  },
  { 
      id: 3, 
      top: 105, 
      left: 180, 
      width: 20, 
      height: 20,
      icon: 'bi:2-circle', 
      name: '普通教室', 
      description: '普通學生在普通的一天在普通的教室上課',
      image:"/Images/zhongshang/9f/9f2.JPEG",
      extraInfo: ''
  },
  { 
      id: 2, 
      top: 40, 
      left: 245, 
      width: 20, 
      height: 20, 
      icon: 'bi:3-circle', 
      name: '普通教室', 
      description: '普通學生在普通的一天在普通的教室上課',
      image:"/Images/zhongshang/9f/9f3.JPEG",
      extraInfo: ''
  },
  { 
      id: 4, 
      top: 45, 
      left: 75, 
      width: 20, 
      height: 20, 
      icon: 'bi:4-circle', 
      name: '財稅系期刊閱讀區', 
      description: '',
      image:"/Images/zhongshang/9f/9f4.JPEG",
      extraInfo: ''
  },
  { 
      id: 5, 
      top: 72, 
      left: 50, 
      width: 20, 
      height: 20, 
      icon: 'bi:5-circle', 
      name: '財稅系系辦', 
      description: '',
      image:"/Images/zhongshang/9f/9f5.JPEG",
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

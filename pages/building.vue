<template>
  <div class="background">
    <h1>大樓介紹</h1>

    <div class="select-wrapper">
      <select v-model="selectedFloor" class="floor-select">
        <option v-for="floor in floors" :key="floor" :value="floor">
          {{ floor }}
        </option>
      </select>
      <div class="icon-trigger" @click="showModal = true">
        <Icon name="icon-park-solid:tips-one" />
      </div>
    </div>

    <div class="components-container">
      <div class="main-component" v-if="currentMainComponent">
        <component :is="currentMainComponent" />
      </div>
      <div class="sub-component" v-if="currentSubComponent">
        <component :is="currentSubComponent" />
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
import { ref, computed, defineAsyncComponent, onMounted } from 'vue';
import { useRoute } from 'vue-router';
import Modal from '~/components/Modal.vue';

const floors = ['1F', '2F', '3F', '4F', '5F', '6F', '7F', '8F', '9F'];
const selectedFloor = ref('1F'); // 默認樓層為 '5F'
const modalTitle = ref('如何查看樓層的信息');
const showModal = ref(true);
const route = useRoute();

const closeModal = () => {
  showModal.value = false;
};

// 根據路由查詢參數初始化 selectedFloor
onMounted(() => {
  if (route.query.floor && floors.includes(route.query.floor)) {
    selectedFloor.value = route.query.floor;
  }
});



// 根據選擇的樓層動態加載主組件
const currentMainComponent = computed(() => {
  console.log('Loading main component for:', selectedFloor.value);
  return defineAsyncComponent(() => import(`~/components/${selectedFloor.value}.vue`));
});

// 根據選擇的樓層動態加載副組件
const currentSubComponent = computed(() => {
  console.log('Loading main component for:', selectedFloor.value);
  return defineAsyncComponent(() => import(`~/components/${selectedFloor.value}1.vue`));
});
</script>

  
  <style scoped>
  .background {
    display: flex;
    flex-direction: column;
    align-items: center;
    height: 100vh;
    padding: 20px;
    box-sizing: border-box;
    background-image: url('/Images/bg.jpg');
    background-size: cover;
    background-position: center;
    background-repeat: no-repeat;
    animation: fadeInBackground 1s ease-in-out;
  }
  
  h1 {
    margin-bottom: 10px;
    font-size: 24px;
    color: white;
    text-shadow: 1px 1px 4px rgba(0, 0, 0, 0.5);
  }
  
  .select-wrapper {
    display: flex;
    align-items: center;
    margin-bottom: 20px;
  }
  
  .floor-select {
    padding: 10px;
    font-size: 16px;
    border-radius: 5px;
    border: 1px solid #ddd;
    background-color: white;
  }
  
  .icon-trigger {
    font-size: 32px;
    cursor: pointer;
    color: black;
    margin-left: 10px;
  }
  
  .icon-trigger:hover {
    color: #ddd;
  }
  
  .components-container {
    display: flex;
    flex-direction: column;
    width: 100%;
    height: 100%;
  }
  
  .main-component,
  .sub-component {
    flex: 1;
    width: 100%;
    box-sizing: border-box;
  }
  
  .main-component {
    margin-bottom: 10px;
  }
  
  @media (max-width: 600px) {
    h1 {
      font-size: 20px;
    }
  
    .background {
      padding: 10px;
    }
  
    .floor-select {
      font-size: 14px;
      padding: 8px;
    }
  }
  </style>
  
<template>
  <div class="background">
    <h1>大樓介紹</h1>

    <div class="select-wrapper">
      <div v-if="selectedFloor === '4F'" >
        <a href="/deintroduce?college=info&dept=im" class="link-to-other-page">前往資管系介紹</a>
      </div>
      <div v-if="selectedFloor === '5F'" >
        <a href="/deintroduce?college=info&dept=im" class="link-to-other-page">前往資工系介紹</a>
      </div>
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
        <p>你可以試著點擊<b>數字</b>來查看一些樓層的區域信息</p>
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
  body{
    background-image: url('/Images/bg.jpg');
  }
  .background {
    display: flex;
    flex-direction: column;
    align-items: center;
    height: 100vh;
    width: 100vw;
    box-sizing: border-box;
    background-size: cover;
    background-position: center;
    background-repeat: no-repeat;
  }
  
  h1 {
    margin-bottom: 10px;
    font-size: 24px;
    color: white;
    text-shadow: 1px 1px 4px rgba(0, 0, 0, 0.5);
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
  
  
  
  .components-container {
    display: flex;
    flex-direction: column;
    width: 100%;
    height: 100%;
  }
  
  .main-component,
  .sub-component {
    flex: 0.4;
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
  
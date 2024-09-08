<template>
  <div>
    <h1>資料抓取嘗試</h1>
    <ul>
      <li v-for="item in data" :key="item.id">
        學生數: {{ item.studentCount }}, 教師數: {{ item.teacherCount }}
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';

const data = ref([]);

const fetchData = async () => {
  try {
    const response = await fetch('http://163.17.135.197/nuxt_api.php'); // 替換為你的 API URL
    if (!response.ok) {
      throw new Error('Network response was not ok');
    }
    const result = await response.json();
    data.value = result;
    console.log(data.value); // 輸出資料到控制台
  } catch (error) {
    console.error('There was a problem with the fetch operation:', error);
  }
};

// 呼叫 fetchData 函數
onMounted(() => {
  fetchData();
});
</script>

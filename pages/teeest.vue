<template>
    <div class="outer-container">
      <!-- 按鈕組部分 -->
      <div class="button-container" :class="{ 'is-moved': isMoved }">
        <div id="container" :class="{ 'is-active': isActive }" class="container">
          <!-- 顯示按鈕 -->
          <div v-for="(item, index) in items" :key="index" class="item" @click="moveAndShowIntroduction(item)">
            <p>{{ item }}</p>
          </div>
          <!-- 中間的圓圈 -->
          <div class="btn" @click="toggle"><p>click</p></div>
        </div>
      </div>
  
      <!-- 介紹頁面部分 -->
      <div class="detail-container" v-if="showDetail">
        <div class="detail">
          <p>{{ currentDetail }}的介紹頁面</p>
          <!-- 根據選中的年級顯示對應的圖片和內文 -->
          <img v-if="currentDetail === '大一'" src="/images/im1.png" alt="大一介紹圖片">
          <img v-if="currentDetail === '大二'" src="/images/im1.png" alt="大二介紹圖片">
          <img v-if="currentDetail === '大三'" src="/images/im2.png" alt="大三介紹圖片">
          <img v-if="currentDetail === '大四'" src="/images/im3.png" alt="大四介紹圖片">
  
          <div v-if="currentDetail === '大一'">
            <h3>課程內容：</h3>
            <p>微積分、計算機概論、會計學、實用日語會話(一)、溝通與簡報、資訊網路</p>
            <h3>學生作品：</h3>
            <p>學生作品展示1</p>
            <h3>活動：</h3>
            <p>新生入學典禮、迎新晚會</p>
          </div>
          <div v-if="currentDetail === '大二'">
            <h3>課程內容：</h3>
            <p>統計學、行銷/生產管理、財務/人力資源管理、資料結構、JAVA程式設計、資料庫管理系統、資訊網路、線性代數、英語會話、實用英文、作業系統、演算法、.NET程式設計、電腦繪圖、Linux系統實務</p>
            <h3>學生作品：</h3>
            <p>學生作品展示2</p>
            <h3>活動：</h3>
            <p>學術講座、企業參訪</p>
          </div>
          <div v-if="currentDetail === '大三'">
            <h3>課程內容：</h3>
            <p>科技英文、物聯網概論、物件導向系統分析、管理資訊系統、企業資源規劃、多媒體系統、人機介面設計、資訊倫理、軟體工程、資料庫專題、跨平台程式設計、巨量資料分析、知識管理、顧客關係管理、網路規劃與管理、伺服器建置與管理、行動程式設計、資訊安全、網際系統設計</p>
            <h3>學生作品：</h3>
            <p>專業項目展示</p>
            <h3>活動：</h3>
            <p>實習機會、校外參訪</p>
          </div>
          <div v-if="currentDetail === '大四'">
            <h3>課程內容：</h3>
            <p>資訊管理實務專題、專業證照、校外實習、產業實習、企業實習、產業專業實習</p>
            <h3>學生作品：</h3>
            <p>畢業作品和研究成果展示</p>
            <h3>活動：</h3>
            <p>畢業典禮、就業輔導</p>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        isActive: false,
        isMoved: false,
        showDetail: false,
        items: ['大一', '大二', '大三', '大四'],
        currentDetail: ''
      };
    },
    methods: {
      toggle() {
        this.isActive = !this.isActive;
      },
      moveAndShowIntroduction(item) {
        this.currentDetail = item;
        this.showDetail = true; // 確保顯示介紹頁面
        this.isMoved = true;
        this.isActive = false; // 收起四散按鈕
      }
      
    },
    mounted() {
    this.$nextTick(() => {
      setTimeout(() => {
        window.dispatchEvent(new Event('resize'));
      }, 500); // 延遲 500 毫秒後觸發 resize 事件
    });
  }
  };
  </script>
  
  <style scoped>
  .outer-container {
    position: relative;
    width: 100%;
    height: 100%;
    display: flex;
    flex-direction: column;
  }
  
  .button-container {
    position: fixed;
    left: 0;
    right: 0;
    bottom: 50%; 
    transition: bottom 0.5s ease;
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 10;
  }
  
  .button-container.is-moved {
    bottom: 0;
  }
  
  .container {
    text-align: center;
    position: relative;
  }
  
  .btn {
    width: 180px;
    height: 180px;
    border-radius: 50%;
    background-color: rgb(168, 211, 241);
    cursor: pointer;
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 2;
  }
  
  .btn p {
    margin: 0;
    font-size: 20px;
    font-weight: bold;
  }
  
  .item {
    position: absolute;
    left: calc(50% - 40px);
    top: 40px;
    width: 80px;
    height: 80px;
    border-radius: 50%;
    background-color: rgb(58, 157, 227);
    display: flex;
    justify-content: center;
    align-items: center;
    cursor: pointer;
    z-index: -1; 
    transition: 0.5s ease;
  }
  
  .item p {
    margin: 0;
    color: white;
    font-size: 16px;
    font-weight: bold;
  }
  
  .is-active .item:nth-child(1) {
    transform: translate(-135px, -45px);
  }
  .is-active .item:nth-child(2) {
    transform: translate(-60px, -135px);
  }
  .is-active .item:nth-child(3) {
    transform: translate(60px, -135px);
  }
  .is-active .item:nth-child(4) {
    transform: translate(135px, -45px);
  }
  
  .detail-container {
    position: relative; /* 改为相对布局 */
    width: 100%;
    padding: 20px;
    text-align: center; /* 将文本内容居中对齐 */
    z-index: 1;
    opacity: 1; 
    transition: opacity 0.5s ease;
    box-sizing: border-box; /* 确保padding不会影响宽度 */
  }
  
  .detail-container .detail {
    font-size: 16px;
    font-weight: normal;
    color: rgb(58, 157, 227);
    line-height: 1.6;
    word-break: break-word; /* 确保长词不会溢出容器 */
    white-space: normal; /* 确保文字正常换行 */
  }
  
  .detail-container img {
    margin-top: 10px;
    max-width: 100%;
    height: auto;
  }
  
  .detail-container h3 {
    margin-top: 15px;
    margin-bottom: 5px;
    font-size: 18px;
    font-weight: bold;
    color: rgb(58, 157, 227);
  }
  
  </style>
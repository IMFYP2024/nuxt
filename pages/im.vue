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
        <div v-if="currentDetail === '師資'">
          <!-- 圖表部分 -->
          <div class="chart-container">
            <h2>師資</h2>
            <canvas id="teacherChart"></canvas>
          </div>
          <h3>{{ selectedTitle }}名單:</h3>
          <!-- 教授列表 -->
          <div class="professor-list">
            <div
              v-for="(professor, index) in selectedProfessors"
              :key="index"
              class="professor-card"
              @click="showProfessorDetail(professor)"
            >
              <img :src="professor.image" alt="Professor Image" class="professor-image" />
              <p>{{ professor.name }}</p>
            </div>
          </div>

          <!-- 彈出視窗 -->
          <div v-if="isModalVisible" class="modal-overlay" @click="closeModal">
            <div class="modal-content" @click.stop>
              <img :src="selectedProfessor.image" alt="Professor Image" class="modal-image" />
              <h3>{{ selectedProfessor.name }}</h3>
              <p>擅長領域: {{ selectedProfessor.expertise }}</p>
              <button @click="closeModal">關閉</button>
            </div>
          </div>
        </div>

        <div v-if="currentDetail === '科系特色'">
          <h2>科系特色</h2>
          <p>學生數:249,教師數:27,上學年度畢業生數:44<br>全系24位專任(案)教師，22位具博士學位，其中教授8位，副教授7位，助理教授9位</p>
          <p>長久以來的教育目標為培育優秀的資訊與管理人才<br>
            四大核心領域「商業智慧」、「智慧物聯」、「巨量資料」及「互動媒體」<br>
            四大課程模組「商業管理」、「資訊系統」、「雲端網路」、「多媒體」
          </p>
          <h3>發展計畫及未來展望</h3>
          <p>•	培養資訊科技與應用之全能專業Plus人才，因應國家資訊建設之需要。<br>
            •	爭取與國內外產官學界之學術交流與建教合作，以增廣學生之視野及實務經驗。<br>
            •	延攬各專業應用領域之師資，推動學術研究與資訊專題實作。<br>
            •	積極參與及舉辦各型國內外學術活動。<br>
            •	籌設小型研討會場所，促進學術交流。<br>
            •	增設專題研究教室。
          </p>
          <h3>畢業</h3>
          <p>本系畢業生可報考資訊管理、資訊工程、數位內容、多媒 本系畢業生可報考資訊管理、資訊工程、數位內容、多媒 體及商管等相關研究所。就業工作可擔任程式設計師、網管工 程師、資料庫管理師、數位內容開發設計工程師、網頁多媒體 設計師、網站設計及管理、多媒體商務整合計畫、電子商務系 統開發、行動應用開發等工作。</p>
        </div>

        <div v-if="currentDetail === '課程'">
          <h2>課程介紹</h2>
          <h3>大一</h3>
          <p>微積分、計算機概論、會計學、經濟學、企業管理、程式設計、實用日語會話(一)、溝通與簡報、資訊網路</p>
          <h3>大二</h3>
          <p>統計學、行銷/生產管理、財務/人力資源管理、資料結構、JAVA程式設計、資料庫管理系統、資訊網路、線性代數、英語會話、實用英文、作業系統、演算法、.NET程式設計、電腦繪圖、Linux系統實務</p>
          <h3>大三</h3>
          <p>科技英文、物聯網概論、物件導向系統分析、管理資訊系統、企業資源規劃、多媒體系統、人機介面設計、資訊倫理、軟體工程、資料庫專題、跨平台程式設計、巨量資料分析、知識管理、顧客關係管理、網路規劃與管理、伺服器建置與管理、行動程式設計、資訊安全、網際系統設計</p>
          <h3>大四</h3>
          <p>電子商務、資訊管理實務專題、供應鏈管理、社群媒體經營、決策支援系統、專案管理、行動服務、人工智慧、雲端服務技術、數位學習、互動展示科技應用</p>
        </div>

        <div v-if="currentDetail === '專題成果'">
          <h2>專題成果</h2>
          <h3>專題是什麼?</h3>
          <p>在升上大三時，有一個特別重要的課程-專題，這不止關乎著你能不能畢業，在製作專題時會獲得巨大的成長，甚至比前兩年所學習到的知識還要更有用更實用，而專題會是可以提早找老師以及組員的，非常建議大家在大一時就好好物色喜歡的組員以及老師，在大二升大三的暑假直接帶著組員跟老師面談，這樣老師也會有充足的時間去訓練你們，或是額外參加比賽等等，而專題製作會結束在大四那一年的12月，有科系舉辦的專題展，也很建議學弟妹可以去看看，看看那一年流行什麼、尋找靈感，去實際的體驗學長姐的作品，對於之後的專題構想也會有幫助!</p>
          <h3>第一名:食在好孕</h3>
          <p></p>
          <h3>第一名:環控偵測防爆IoT系統</h3>
          <p></p>
          <h3>第二名:以數位轉型改善偏鄉學生程式與資訊科技素養</h3>
          <p></p>
          <h3>第二名:詐騙護手</h3>
          <p></p>
          <h3>第三名:蘋狗</h3>
          <p></p>
          <h3>第三名:智能垃圾自動分類機</h3>
          <p></p>
        </div>

      </div>
    </div>
  </div>
</template>

<script>
import { Chart, registerables } from 'chart.js';
import { nextTick } from 'vue';
Chart.register(...registerables);

export default {
  data() {
    return {
      isActive: false,
      isMoved: false,
      showDetail: false,
      isModalVisible: false, // 控制彈出視窗的顯示
      selectedProfessor: null, // 保存點擊的老師詳細信息
      items: ['科系特色', '師資', '課程', '專題成果'],
      currentDetail: '',
      activeIndex: null, // 保存被點擊的長條索引
      selectedTitle: '', // 保存被點擊的類別名稱
      selectedProfessors: [], // 保存顯示的教授列表
      professorLists: {
        教授: [
          { name: '黃秀美', image: 'path_to_image1.jpg', expertise: '數據分析' },
          { name: '陳大仁', image: 'path_to_image2.jpg', expertise: '物聯網' },
          { name: '蕭國倫', image: 'path_to_image3.jpg', expertise: '資訊安全' },
          { name: '柯志坤', image: 'path_to_image4.jpg', expertise: '人工智慧' },
          { name: '黃天麒', image: 'path_to_image5.jpg', expertise: '軟體工程' },
          { name: '連俊瑋', image: 'path_to_image6.jpg', expertise: '巨量資料' },
          { name: '林真伊', image: 'path_to_image7.jpg', expertise: '雲端計算' },
          { name: '王健亞', image: 'path_to_image8.jpg', expertise: '資料庫管理' }
        ],
        副教授: [
          { name: '王淑玲', image: 'path_to_image9.jpg', expertise: '企業管理' },
          { name: '廖士寬', image: 'path_to_image10.jpg', expertise: '電腦繪圖' }
        ],
        助理教授: [
          { name: '羅張瓊誼', image: 'path_to_image11.jpg', expertise: '程式設計' }
        ]
      }
    };
  },
  methods: {
    toggle() {
      this.isActive = !this.isActive;
    },
    moveAndShowIntroduction(item) {
      this.currentDetail = item;
      this.showDetail = true;
      this.isMoved = true;
      this.isActive = false;

      // 初始化圖表當選擇"師資"時
      if (item === '師資') {
        this.initChart();
      }
    },
    initChart() {
      const chartData = {
        labels: ['教授', '副教授', '助理教授'],
        datasets: [{
          label: '人數',
          data: [8, 8, 9],
          backgroundColor: ['rgba(75, 192, 192, 0.5)', 'rgba(54, 162, 235, 0.5)', 'rgba(104, 0, 145, 0.2)'],
          borderColor: ['rgba(75, 192, 192, 1)', 'rgba(54, 162, 235, 1)', 'rgba(104, 0, 145, 0.1)'],
          borderWidth: 0
        }]
      };

      nextTick(() => {
        const ctx = document.getElementById('teacherChart').getContext('2d');
        this.chart = new Chart(ctx, {
          type: 'bar',
          data: chartData,
          options: {
            responsive: true,
            plugins: {
              legend: {
                display: false // 隱藏圖例
              },
              tooltip: {
                enabled: true // 啟用 tooltip
              }
            },
            onClick: (event, elements) => {
              if (elements.length > 0) {
                const index = elements[0].index;
                this.selectCategory(index, chartData);
              }
            }
          }
        });

        // 自動選擇教授並顯示名單
        this.selectCategory(0, chartData); // 0 是教授的索引
         const tooltip = this.chart.tooltip;
      this.chart.setActiveElements([{ datasetIndex: 0, index: 0 }]);
      
      tooltip.setActiveElements([{ datasetIndex: 0, index: 0 }], {
        x: this.chart.getDatasetMeta(0).data[0].x,
        y: this.chart.getDatasetMeta(0).data[0].y,
      });
  
  tooltip.update();
  this.chart.update();
      });
    },
    selectCategory(index, chartData) {
      this.activeIndex = index;
      const category = chartData.labels[index];
      this.selectedTitle = category;
      this.selectedProfessors = this.professorLists[category];

      this.updateChartHighlight(chartData);
    },
    updateChartHighlight(chartData) {
      const backgroundColor = ['rgba(75, 192, 192, 0.5)', 'rgba(54, 162, 235, 0.5)', 'rgba(104, 0, 145, 0.2)'];
      const borderColor = ['rgba(75, 192, 192, 1)', 'rgba(54, 162, 235, 1)', 'rgba(104, 0, 145, 0.44)'];

      if (this.activeIndex !== null) {
        backgroundColor[this.activeIndex] = 'rgba(255, 99, 132, 1)';
        borderColor[this.activeIndex] = 'rgba(255, 99, 132, 1)';
      }

      chartData.datasets[0].backgroundColor = backgroundColor;
      chartData.datasets[0].borderColor = borderColor;
      this.chart.update();
    },
    showProfessorDetail(professor) {
      this.selectedProfessor = professor;
      this.isModalVisible = true;
    },
    closeModal() {
      this.isModalVisible = false;
    }
  },
  mounted() {
    setTimeout(() => {
      this.isActive = true;
    });
    this.moveAndShowIntroduction(this.items[0]);
  },
  beforeUnmount() {
    if (this.chart) {
      this.chart.destroy();
    }
  }
};
</script>

<style scoped>
.outer-container {
  position: relative;
  width: 100vw;
  height: 100vh;
  display: flex;
  flex-direction: column;
}

.button-container {
  position: fixed;
  left: 0;
  right: 0;
  bottom: 0;
  transition: bottom 0.5s ease;
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 10;
}

.container {
  text-align: center;
  position: relative;
}

.btn {
  width: 120px;
  height: 120px;
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
  transform: translate(-105px, -45px);
}
.is-active .item:nth-child(2) {
  transform: translate(-45px, -115px);
}
.is-active .item:nth-child(3) {
  transform: translate(45px, -115px);
}
.is-active .item:nth-child(4) {
  transform: translate(105px, -45px);
}

.detail-container {
  position: relative;
  width: 100%;
  padding: 20px;
  text-align: center;
  z-index: 1;
  opacity: 1;
  transition: opacity 0.5s ease;
  box-sizing: border-box;
  height: calc(85vh - 100px);
  overflow-y: auto;
}

.professor-list {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
}

.professor-card {
  width: 150px;
  text-align: center;
  cursor: pointer;
}

.professor-image {
  width: 100%;
  height: auto;
  border-radius: 50%;
}

.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.7);
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal-content {
  background-color: white;
  padding: 20px;
  border-radius: 10px;
  text-align: center;
  position: relative;
}

.modal-image {
  width: 100px;
  height: 100px;
  border-radius: 50%;
}

button {
  margin-top: 20px;
  padding: 10px 20px;
  background-color: #007bff;
  color: white;
  border: none;
  cursor: pointer;
  border-radius: 5px;
}
</style>

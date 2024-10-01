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
        <h1>資訊管理系</h1>
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
          <h2>課程</h2>
          <div class="">
    <!-- Custom dropdown for selecting academic year -->
    <div class="year-selector">
      <div class="custom-select" @click="toggleDropdown">
        <div class="selected-option">
          {{ selectedYear }}
          <i class="fi fi-rr-caret-down"></i> <!-- Added icon here -->
        </div>
        <div class="options" v-if="isDropdownOpen">
          <div
            v-for="year in years"
            :key="year"
            @click.stop="selectYear(year)"
            class="option"
          >
            {{ year }}
          </div>
        </div>
      </div>
    </div>

    <!-- Course list -->
    <div class="course-list">
      <div v-for="(course, index) in currentCourses" :key="index" class="course-item">
    <div class="course-required">
      <img :src="course.type === '必修' ? requiredIcon : electiveIcon" alt="course type icon" class="course-type-icon">
    </div>
    <div class="course-name">
      {{ course.name }}
        <i class="fi fi-br-info" @click="showCreditModal('一、二年級一學期不能小於16學分，三、四年級一學期不可以小於9學分，全年級一學期不可以多餘25')"></i>
      </div>
      <div class="course-credits">{{ course.credits }}</div>
      </div>
    </div>
  <div v-if="isCreditModalVisible" class="modal-overlay" @click="closeCreditModal">
    <div class="modal-content" @click.stop>
      <p>{{ creditMessage }}</p>
      <button @click="closeCreditModal">關閉</button>
    </div>
  </div>
    <!-- Total credits summary -->
    <div class="credits-summary">
      <div>
        <img :src="requiredIcon" alt="required icon" class="course-type-icon">
        {{ totalRequiredCredits }} 學分
        <img :src="electiveIcon" alt="elective icon" class="course-type-icon">
        {{ totalElectiveCredits }} 學分
      </div>
    </div>
  </div>
        </div>

        <div v-if="currentDetail === '專題競賽'">
          <div class="button-toggle">
      <button :class="{ active: activeSection === '專題' }" @click="toggleSection('專題')">專題</button>
      <button :class="{ active: activeSection === '競賽' }" @click="toggleSection('競賽')">競賽</button>
    </div>

    <!-- Swiper 輪播圖部分 -->
    <div class="swiper-container">
      <client-only>
        <swiper
          
          :slides-per-view="1.3"
          :space-between="20"
          centeredSlides="true"
          :navigation="false"
          loop="true"
          grab-cursor="true"
          v-if="activeSection === '專題'"
        >
          <!-- 專題卡片樣式 -->
          <swiper-slide v-for="(item, index) in topics" :key="index" class="carousel-item">
            <div class="card">
              <img :src="item.image" alt="Project Image" class="carousel-image" />
              <div class="card-content">
                <h3>{{ item.title }}</h3>
                <p>名次: {{ item.rank }}</p>
                <p>老師: {{ item.teacher }}</p>
                <p>專題成員: {{ item.student }}</p>
              </div>
            </div>
          </swiper-slide>
        </swiper>
      </client-only>

      <client-only>
        <swiper
          
          :slides-per-view="1.3"
          :space-between="20"
          centeredSlides="true"
          :navigation="false"
          loop="true"
          grab-cursor="true"
          v-if="activeSection === '競賽'"
        >
          <!-- 競賽卡片樣式 -->
          <swiper-slide v-for="(item, index) in competitions" :key="index" class="carousel-item">
            <div class="card">
              <img :src="item.image" alt="Competition Image" class="carousel-image" />
              <div class="card-content">
                <h3>{{ item.title }}</h3>
                <p>名次: {{ item.rank }}</p>
                <p>老師: {{ item.teacher }}</p>
                <p>競賽成員: {{ item.student }}</p>
              </div>
            </div>
          </swiper-slide>
        </swiper>
      </client-only>
    </div>
        </div>

      </div>
    </div>
  </div>
</template>

<script>
import { Chart, registerables } from 'chart.js';
import { nextTick } from 'vue';
Chart.register(...registerables);
import { Swiper, SwiperSlide } from 'swiper/vue';
import 'swiper/swiper-bundle.min.css'; 

export default {
  components: {
    Swiper,
    SwiperSlide
  },
  data() {
    return {
      isCreditModalVisible: false, // 控制彈出視窗的顯示
      creditMessage: '',
      isActive: false,
      isMoved: false,
      showDetail: false,
      isModalVisible: false, // 控制彈出視窗的顯示
      selectedProfessor: null, // 保存點擊的老師詳細信息
      items: ['科系特色', '師資', '課程', '專題競賽'],
      currentDetail: '',
      activeIndex: null, // 保存被點擊的長條索引
      selectedTitle: '', // 保存被點擊的類別名稱
      selectedProfessors: [], // 保存顯示的教授列表
      professorLists: {
        教授: [
          { name: '姜老師', image: '/Images/teacher.png', expertise: '資料探勘、數據分析、統計與計量方法、派翠網路、生物醫療、訊號處理' },
          { name: '黃老師', image: '/Images/teacher.png', expertise: '數位學習、虛擬實境與擴增實境、智慧型資訊系統、電子商務' },
          { name: '陳老師', image: '/Images/teacher.png', expertise: '網路協定與演算法、物聯網、功耗感知、即時系統、網際網路應用、無線網路、雲端運算' },
          { name: '蕭老師', image: '/Images/teacher.png', expertise: '電子商務、網路行銷、電子化企業、知識管理、科技管理、管理決策' },
          { name: '柯老師', image: '/Images/teacher.png', expertise: '資訊系統、資料分析、行動運算、知識管理與探勘' },
          { name: '黃老師', image: '/Images/teacher.png', expertise: '數位學習、創造力培育、知識工程、教育科技、創客教育' },
          { name: '連老師', image: '/Images/teacher.png', expertise: '資訊管理、電子商務、電子化企業、服務科學與設計思考、醫療資訊管理' },
          { name: '林老師', image: '/Images/teacher.png', expertise: '料探勘、推薦系統、社會網絡分析' },
          { name: '王老師', image: '/Images/teacher.png', expertise: '工作排程、影像處理、電腦視覺、演算法、資料庫系統、專利檢索' }
        ],
        副教授: [
          { name: '王老師', image: '/Images/teacher.png', expertise: '行動加值與應用、智慧型人機介面、雲端與網路服務系統、多變量分析、企業電子化系統' },
          { name: '廖老師', image: '/Images/teacher.png', expertise: '資訊視覺化、電腦視覺、計算機圖學' },
          { name: '駱老師', image: '/Images/teacher.png', expertise: '網路安全、資訊網路、網路應用、電子商務安全、醫療資訊安全' },
          { name: '張老師', image: '/Images/teacher.png', expertise: '人力資源、職涯規劃、行銷管理' },
          { name: '何老師', image: '/Images/teacher.png', expertise: '專長及研究領域：組織行為、領導、建言行為、人力資源管理、動機取向' },
          { name: '林老師', image: '/Images/teacher.png', expertise: '元宇宙學習應用、多媒體應用與開發、數位學習與應用、雲端系統應用與虛擬化、網路安全、行動通訊、計算機網路、智慧物聯網、AI教學應用' },
          { name: '侯老師', image: '/Images/teacher.png', expertise: '數位學習與大數據分析、程式設計與應用、統計學、多變量分析' },
          { name: '林老師', image: '/Images/teacher.png', expertise: '電子商務、數位行銷、社群媒體、消費者行為、數量方法分析' }
        ],
        助理教授: [
          { name: '羅老師', image: '/Images/teacher.png', expertise: '科技教育、資訊管理' },
          { name: '邱老師', image: '/Images/teacher.png', expertise: '資訊安全、網路安全、影像處理' },
          { name: '許老師', image: '/Images/teacher.png', expertise: '資料庫系統、資料探勘、資料分析與處理、推薦系統' },
          { name: '戴老師', image: '/Images/teacher.png', expertise: '多媒體領域(含3D、VR/AR)、資訊系統' },
          { name: '周老師', image: '/Images/teacher.png', expertise: '科技教育、擴增實境、人機互動設計' },
          { name: '曾老師', image: '/Images/teacher.png', expertise: '教育大數據、測驗統計與評量、數位學習' },
          { name: '李老師', image: '/Images/teacher.png', expertise: '人工智慧、創新顯示互動設計、眼動偵測分析、情感運算、生理資訊感測、行動運算、異質系統整合與開發' },
          { name: '林老師', image: '/Images/teacher.png', expertise: '新興科技應用、資訊管理、虛實整合、問題導向學習應用、STEAM跨領域教學策略' },
          { name: '陳老師', image: '/Images/teacher.png', expertise: '資訊安全、程式設計、資訊系統管理、網路管理、資料庫管理' }
        ]
      },
      selectedYear: '請選擇年級', // Default selected year
      isDropdownOpen: false, // Control the dropdown state
      years: ['大一', '大二', '大三', '大四'], // Dropdown options
      currentCourses: [], // List of courses for the selected year
      courses: {
        大一: [
          { type: '必修', name: '微積分', credits: 3 },
          { type: '必修', name: '計算機概論', credits: 3 },
          { type: '必修', name: '會計學', credits: 2 },
          { type: '必修', name: '經濟學', credits: 2 },
          { type: '必修', name: '企業管理', credits: 2 },
          { type: '必修', name: '程式設計', credits: 2 },
          { type: '選修', name: '實用日語會話(一)', credits: 2 },
          { type: '選修', name: '溝通與簡報', credits: 3 },
          { type: '選修', name: '資訊網路', credits: 3 },
        ],
        大二: [
          { type: '必修', name: '統計學', credits: 3 },
          { type: '必修', name: '行銷/生產管理', credits: 3 },
          { type: '必修', name: '財務/人力資源管理', credits: 3 },
          { type: '必修', name: '資料結構', credits: 3 },
          { type: '必修', name: 'JAVA程式設計', credits: 3 },
          { type: '必修', name: '資料庫管理系統', credits: 3 },
          { type: '必修', name: '資訊網路', credits: 2 },
          { type: '選修', name: '線性代數', credits: 3 },
          { type: '選修', name: '英語會話', credits: 2 },
          { type: '選修', name: '實用英文', credits: 2 },
          { type: '選修', name: '作業系統', credits: 3 },
          { type: '選修', name: '演算法', credits: 3 },
          { type: '選修', name: '.NET程式設計', credits: 3 },
          { type: '選修', name: '電腦繪圖', credits: 3 },
          { type: '選修', name: 'Linux系統實務', credits: 3 },
        ],
        大三: [
          { type: '必修', name: '科技英文', credits: 3 },
          { type: '必修', name: '物聯網概論', credits: 3 },
          { type: '必修', name: '物件導向系統分析', credits: 3 },
          { type: '必修', name: '管理資訊系統', credits: 3 },
          { type: '必修', name: '企業資源規劃', credits: 3 },
          { type: '必修', name: '多媒體系統', credits: 3 },
          { type: '必修', name: '人機介面設計', credits: 3 },
          { type: '選修', name: '資訊倫理', credits: 3 },
          { type: '選修', name: '軟體工程', credits: 3 },
          { type: '選修', name: '資料庫專題', credits: 2 },
          { type: '選修', name: '跨平台程式設計', credits: 3 },
          { type: '選修', name: '巨量資料分析', credits: 2 },
          { type: '選修', name: '知識管理', credits: 2 },
          { type: '選修', name: '顧客關係管理', credits: 3 },
          { type: '選修', name: '網路規劃與管理', credits: 3 },
          { type: '選修', name: '伺服器建置與管理', credits: 3 },
          { type: '選修', name: '行動程式設計', credits: 3 },
          { type: '選修', name: '資訊安全', credits: 3 },
          { type: '選修', name: '網際系統設計', credits: 3 },
        ],
        大四: [
          { type: '必修', name: '電子商務', credits: 3 },
          { type: '選修', name: '資訊管理實務專題', credits: 3 },
          { type: '選修', name: '供應鏈管理', credits: 3 },
          { type: '選修', name: '社群媒體經營', credits: 3 },
          { type: '選修', name: '決策支援系統', credits: 3 },
          { type: '選修', name: '專案管理', credits: 3 },
          { type: '選修', name: '行動服務', credits: 3 },
          { type: '選修', name: '人工智慧', credits: 3 },
          { type: '選修', name: '雲端服務技術', credits: 3 },
          { type: '選修', name: '數位學習', credits: 3 },
          { type: '選修', name: '互動展示科技應用', credits: 3 },
        ],
      },
      requiredIcon: '/Images/IMG_0047.png', // Replace with the path to the required icon
      electiveIcon: '/Images/IMG_0046.png',
        activeSection: '專題',  // 默認顯示 "專題" 部分
      topics: [
        { rank: '第一名', title: '食在好孕', teacher: '蔣老師', student: '王曉明', image: '/Images/test.png' },
        { rank: '第二名', title: '環控偵測防爆IoT', teacher: '陳老師', student: '小雯', image: '/Images/test.png' },
        { rank: '第三名', title: '詐騙護手', teacher: '王老師', student: '阿呆', image: '/Images/test.png' },
        { rank: '第四名', title: '蘋狗', teacher: '王老師', student: '嘿嘿', image: '/Images/test.png' },
        { rank: '第五名', title: 'boombom', teacher: '王老師', student: '哇哇', image: '/Images/test.png' },
      ],
      competitions: [
        { rank: '第一名', title: '食在好孕', teacher: '蔣老師', student: '王曉明', image: '/Images/test.jpg' },
        { rank: '第二名', title: '環控偵測防爆IoT', teacher: '陳老師', student: '小雯', image: '/Images/test.jpg' },
        { rank: '第三名', title: '詐騙護手', teacher: '王老師', student: '阿呆', image: '/Images/test.jpg' },
      ],
    };
  },
  computed: {
    totalRequiredCredits() {
      return this.currentCourses
        .filter(course => course.type === '必修')
        .reduce((total, course) => total + course.credits, 0);
    },
    totalElectiveCredits() {
      return this.currentCourses
        .filter(course => course.type === '選修')
        .reduce((total, course) => total + course.credits, 0);
    },
  },
  methods: {
     showCreditModal(message) {
    this.creditMessage = message;
    this.isCreditModalVisible = true;
    },
    closeCreditModal() {
      this.isCreditModalVisible = false;
    },
    toggle() {
      this.isActive = !this.isActive;
    },
    toggleSection(section) {
    this.activeSection = section;
    this.$nextTick(() => {
      //this.$refs.mySwiper.update(); // 更新 Swiper 佈局
    });
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
          data: [9, 8, 9],
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
    },
    toggleDropdown() {
      this.isDropdownOpen = !this.isDropdownOpen;
    },
    selectYear(year) {
      this.selectedYear = year;
      this.updateCourseList();
      this.isDropdownOpen = false; // Close the dropdown after selecting a year
    },
    updateCourseList() {
      this.currentCourses = this.courses[this.selectedYear];
    },
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
@import url('https://cdn.jsdelivr.net/npm/@flaticon/flaticon-uicons/css/all/all.css');
.outer-container {
  align-items: center;
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
  height: calc(95vh - 100px);
  overflow: auto;
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
.options {
  position: absolute;
  top: 100%;
  left: 50%;
  transform: translateX(-50%);
  background-color: #ff0;
  border-radius: 5px;
  overflow: hidden;
  z-index: 1000;
  width: 200px;
  }
.custom-select {
   width: 200px;
  background-color: #ff0;
  padding: 10px;
  text-align: center;
  border-radius: 5px;
  cursor: pointer;
  user-select: none;
  display: flex;
  align-items: center;
  justify-content: center;
}
.option {
  padding: 10px;
  text-align: center;
  cursor: pointer;
}
.course-type-icon {
  width: 40px; /* Adjusted size for better alignment */
  height: 40px;
}
.course-item {
  display: flex;
  align-items: center;
  justify-content: center;
  padding: -10px 0;
  border-bottom: 1px solid #ccc;
  width: 80%;
  max-width: 400px;
}
.course-list {
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  
}
.selected-option {
  font-weight: bold;
}
.year-selector {
  margin-bottom: 20px;
  position: relative;
  display: flex;
  justify-content: center; /* Center the dropdown *//* Center the dropdown */
}
.outer-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: -50%;
}
.course-name {
  flex-grow: 1;
  text-align: center; /* Center align the course name */
}

.course-credits {
  text-align: center; /* Center align the credits */
  font-weight: bold;
  min-width: 30px; /* Ensure fixed space for credit numbers */
}

.credits-summary {
  margin-top: 20px;
  text-align: center;
}


.button-toggle {
  display: flex;
  justify-content: center;
  margin-bottom: 20px;
}

.button-toggle button {
  margin: 0 10px;
  padding: 10px 20px;
  background-color: lightgray;
  border: none;
  border-radius: 10px;
}

.button-toggle button.active {
  background-color: #007bff;
  color: white;
}

.swiper-container {
  width: 100%;
  overflow: hidden;
}
.swiper-slide {
  width: auto; /* 自動適應內容 */
  display: flex;
  justify-content: center;
}
.carousel-item {
  display: flex;
  justify-content: center;
  width: 100%;
  }


.card {
   background-color: #fff;
  border-radius: 20px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  overflow: hidden;
  width: 250px; /* 根據需要調整卡片寬度 */
  text-align: center;
  margin: 0 auto;
}

.carousel-image {
  width: 100%;
  height: 200px;
  object-fit: cover;
}

.card-content {
  padding: 15px;
}

.card-content h3 {
  font-size: 1.5rem;
  margin-bottom: 10px;
}

.card-content p {
  margin: 5px 0;
  font-size: 1rem;
  color: #555;
}
.button-toggle {
  display: flex;
  justify-content: center;
  margin-bottom: 20px;
}

.button-toggle button {
  margin: 0 10px;
  padding: 10px 20px;
  background-color: lightgray;
  border: none;
  border-radius: 10px;
  cursor: pointer;
}

.button-toggle button.active {
  background-color: #007bff;
  color: white;
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
  z-index: 1000;
}

.modal-content {
  background-color: white;
  padding: 20px;
  border-radius: 10px;
  text-align: center;
  position: relative;
}
</style>

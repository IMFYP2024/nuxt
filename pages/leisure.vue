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
        <h1>休閒事業經營系</h1>
        <div v-if="currentDetail === '師資'">
          <!-- 圖表部分 -->
          <div class="chart-container card">
            <h2><br>師資<i class="fi fi-rr-interrogation custom-icon1" @click="showInfoModal"></i></h2>
            <canvas id="teacherChart"></canvas>
          </div>
          <div v-if="isInfoModalVisible" class="modal-overlay" @click="closeInfoModal">
          <div class="modal-content" @click.stop>
            <span class="close-icon" @click="closeInfoModal">×</span>
            <p>可以與統計圖互動哦!</p>
          </div>
        </div>
          <div class="card" style="margin-top:20px">
          <h3><br>{{ selectedTitle }}名單:</h3>
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
          </div>
          <!-- 彈出視窗 -->
          <div v-if="isModalVisible" class="modal-overlay" @click="closeModal">
            <div class="modal-content" @click.stop>
              <img :src="selectedProfessor.image" alt="Professor Image" class="modal-image" />
              <h3>{{ selectedProfessor.name }}</h3>
              <p>擅長領域: {{ selectedProfessor.expertise }}</p>
              <button @click="closeModal">關閉</button>
            </div>
          </div><div class="air"></div>
        </div>

        <div v-if="currentDetail === '科系特色'"> 
       <!--  <i class="fi fi-rr-dot-pending menu-icon" @click="toggleSidebar"></i>

         側邊欄 
        <div class="sidebar" :class="{ 'is-open': isSidebarOpen }">
          <div class="close-btn" @click="toggleSidebar"></div>
          <ul>
            <li @click="scrollToSection('科系特色')">科系特色</li>
            <li @click="scrollToSection('發展計畫及未來展望')">發展計畫及未來展望</li>
            <li @click="scrollToSection('畢業')">畢業</li>
          </ul>
        </div>
       科系特色區塊 -->
      
        
        <div id="科系特色" class="card1">
          <h3>科系特色</h3>
          <div class="stats-container">
            <div class="stat-item">
              <p>科系/全校學生數</p><br>
              <h2>{{ studentCount }}</h2><p> / 5,528</p>
            </div>
            <div class="stat-item">
              <p>科系老師數</p><br>
              <h2>{{ teacherCount }}</h2>
            </div>
            <div class="stat-item">
              <p>上學年度畢業生數</p><br>
              <h2>{{ graduateCount }}</h2>
            </div>
            <div class="stat-item">
              <p>具博士學位教師</p><br>
              <h2>{{ phdCount }}</h2>
            </div>
          </div>
          <p class="description"> 本系結合本校商管專長之傳統，針對「休閒遊憩經營」與「觀光與餐旅經營」等兩大休閒產業領域，系統性地培育休閒事業之專業管理人才。</p>
          <img src="/Images/lei.jpg" class="imin" alt="Education Objective">

        </div>
      


      <!-- 發展計畫及未來展望區塊 -->
       <div id="發展計畫及未來展望" class="card2">
        <h3>發展計畫及未來展望</h3>
        <ul>
          <li>積極推動國內外<span class="highlight">企業實習</span>，增加學生<span class="highlight">實務經驗</span>及<span class="highlight">國際觀</span>，以利<span class="highlight">未來理論與實務</span>之整合。</li>
          <li>提供師生教學與實習之場所，結合<span class="highlight">實務操作</span>，可培養實務界所需之人才，提高學生<span class="highlight">就業</span>之競爭力。</li>
          <li>強調<span class="highlight">實務教學</span>、加強<span class="highlight">技術學習</span>、實施<span class="highlight">專題製作</span>、落實<span class="highlight">證照輔導制度</span>及推展校外<span class="highlight">實習課程</span>等。</li>
          
        </ul>
      </div>

      <!-- 畢業區塊 -->
        <div id="畢業" class="card3">
          <h3>發展方向</h3>
          
          <!-- 研究所一欄 -->
        

          <!-- 就業工作一欄 -->
          <div class="job-section">
            
            <ul>
              <li>觀光旅館業</li>
              <li>會議展覽業</li>
              <li>商務旅館</li>
              <li>俱樂部</li>
              <li>旅行社</li>
              <li>商圈規劃顧問</li>
              <li>旅運業等相關城市休閒事業機構從事規劃、管理、開發、服務</li>
            </ul>
            
          </div>
        </div>
        <div class="air"></div>
      </div>
      </div>


        <div v-if="currentDetail === '課程'">
          <div class="card" style="height:auto; margin-top:10px">

          <h2><br>課程</h2>
          <i class="fi fi-br-info custom-icon" @click="showCreditModal"></i>
          <div v-if="isCreditModalVisible" class="modal-overlay" @click="closeCreditModal">
          <div class="modal-content" @click.stop>
            <p style=" text-align: left " v-html="creditMessage"></p><!-- 顯示訊息 -->
            
          </div>
        </div>
          <div class="">
    <!-- Custom dropdown for selecting academic year -->
          <div class="year-selector">
            <div class="custom-select" @click="toggleDropdown">
              <div class="selected-option" style="z-index: 10">
                {{ selectedYear }}
                <i class="fi fi-rr-caret-down">
                  
                  </i> <!-- Added icon here -->
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
        
      </div>
      <div class="course-credits">{{ course.credits }}</div>
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
  </div><div class="air"></div>
        
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
            <div class="card4">
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
            <div class="card4">
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
    </div><div class="air"></div>
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
   watch: {
    // 監聽 currentDetail 的變化
    currentDetail(newValue) {
      if (newValue === '科系特色') {
        this.animateCount('studentCount', 173, 2000); // 動畫持續 2 秒
        this.animateCount('teacherCount', 12, 2000);
        this.animateCount('graduateCount', 47, 2000);
        this.animateCount('phdCount', 8, 2000);
      }
    },
  },
  el: '#app',
  data() {
    return {
      isInfoModalVisible: false,
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
          { name: '鄭瑞昌', image: '/Images/leisure/1.jpg', expertise: '會展管理、財務管理、主題樂園管理、、博弈管理產業、休閒健康產業、休閒創業、、投資郵輪產業、休閒觀光管理、人力資源管理' },
          { name: '張李淑容', image: '/Images/leisure/2.jpg', expertise: '會計、經濟、產業分析、觀光地理、遊程規劃' },
          { name: '劉慧蓉', image: '/Images/leisure/3.jpg', expertise: '餐飲經營與管理、餐飲衛生安全管理、消費心理、服務業管理' },
          { name: '顏昌華', image: '/Images/leisure/4.jpg', expertise: '觀光事業管理、旅館管理、旅行業行銷與管理' },
          { name: '黃健哲', image: '/Images/leisure/5.jpg', expertise: '管理學、休閒產業分析、人力資源管理、雲端系統' },
          
        ],
        副教授: [
          { name: '洪淑宜', image: '/Images/leisure/6.jpg', expertise: '消費者行為(含消費文化)、整合行銷傳播(含行銷管理)、文化創意產業與觀光、公共關係管理' },
          { name: '陳元陽', image: '/Images/leisure/7.jpg', expertise: '休閒遊憩資源調查與規劃、公園遊憩區規劃設計、戶外遊憩研、究、國家公園經營、生態觀光、森林遊樂、休閒社會學' },
          { name: '鄧秀玉', image: '/Images/leisure/8.jpg', expertise: '旅行業資訊應用 、會議展覽管理、餐旅服務業管理' },
          { name: '林銘昌', image: '/Images/leisure/9.png', expertise: '經濟分析、休閒產業研究、創業管理、鄉村旅遊規劃' },
                   
        ],
        助理教授: [
          { name: '李安珮', image: '/Images/leisure/10.jpg', expertise: '管理學、行銷學、財務領域、國際企管、休閒應用英語、國際禮儀' },
          { name: '楊道欣', image: '/Images/leisure/11.jpg', expertise: '飲料調製、創意調酒、餐旅服務、飲調實務、咖啡調配與拉花技術、雞尾酒調製、吧檯管理實務、選手培訓、餐飲服務技術' },
          
        ]
      },
      selectedYear: '請選擇年級', // Default selected year
      isDropdownOpen: false, // Control the dropdown state
      years: ['大一', '大二', '大三', '大四'], // Dropdown options
      currentCourses: [], // List of courses for the selected year
      courses: {
        大一: [
          { type: '必修', name: '經濟學', credits: 3 },
          { type: '必修', name: '會計學', credits: 3 },
          { type: '必修', name: '英文', credits: 3 },
          { type: '必修', name: '國文', credits: 2 },
          { type: '必修', name: '體育', credits: 0 },
          { type: '必修', name: '管理學', credits: 3 },
          { type: '必修', name: '休閒概論', credits: 3 },
          { type: '必修', name: '程式設計與應用', credits: 2 },
          { type: '必修', name: '大學之道', credits: 2 },
          { type: '必修', name: '服務與學習', credits: 1 },
          { type: '必修', name: '學涯規劃', credits: 1 },
          { type: '必修', name: '網頁設計', credits: 2 },          
          { type: '選修', name: '休閒應用日語（一）', credits: 2 },
          { type: '選修', name: '人際溝通技巧', credits: 2 },
          { type: '選修', name: '文書處理', credits:1 },
          { type: '選修', name: '餐飲營養學', credits: 3 },
          { type: '選修', name: '休閒社會學', credits: 2 },
          { type: '選修', name: '國際禮儀', credits: 2 },
          { type: '選修', name: '休閒應用日語（二）', credits: 2 },
        ],
        大二: [
          { type: '必修', name: '統計學', credits: 3 },
          { type: '必修', name: '生涯運動', credits: 0 },
          { type: '必修', name: '行銷管理', credits: 3 },
          { type: '必修', name: '人力資源管理', credits: 3 },
          { type: '必修', name: '休閒應用英語（一）', credits: 2 },
          { type: '必修', name: '臺灣開發史', credits: 2 },
          { type: '必修', name: '財務管理', credits: 3 },
          { type: '必修', name: '休閒服務業管理', credits: 3 },
          { type: '必修', name: '休閒事業公共關係學', credits: 3 },
          { type: '必修', name: '休閒心理與消費行為', credits: 3 },
          { type: '必修', name: '休閒應用英語（二）', credits: 2 },
          { type: '必修', name: '民主憲政與法治', credits: 3 },
          { type: '選修', name: '管理資訊系統', credits: 3 },
          { type: '選修', name: '解說導覽概論', credits: 3 },
          { type: '選修', name: '觀光地理', credits: 3 },
          { type: '選修', name: '餐廳經營管理', credits: 3 },
          { type: '選修', name: '會議展覽管理', credits: 3 },
          { type: '選修', name: '旅行業資訊應用', credits: 3 },
          { type: '選修', name: '食物學原理', credits: 3 },
          { type: '選修', name: '觀光行政與法規', credits: 3 },
          { type: '選修', name: '電子商務與網路行銷應用', credits: 3 },
          { type: '選修', name: '國家公園與生態觀光', credits: 3 },
          { type: '選修', name: '旅館管理與實務', credits: 3 },
          { type: '選修', name: '觀光事業概論', credits: 2 },
          
        ],
        大三: [
          { type: '必修', name: '遊憩資源調查與規劃', credits: 3 },
          { type: '必修', name: '休閒產業分析', credits: 3 },
          { type: '必修', name: '休閒健康事業經營個案分析', credits: 3 },
          { type: '必修', name: '休閒研究方法', credits: 3 },
          { type: '必修', name: '應用文與習作', credits: 2 },
          { type: '必修', name: '職涯規劃', credits: 1 },
          { type: '選修', name: '社區與休閒', credits: 3 },
          { type: '選修', name: '餐旅成本分析', credits: 3 },
          { type: '選修', name: '飲務管理', credits: 3 },
          { type: '選修', name: '餐飲衛生與安全', credits: 3 },
          { type: '選修', name: '休閒事業整合行銷創意企劃	', credits: 3 },
          { type: '選修', name: '巨量資料概論', credits: 3 },
          { type: '選修', name: '領隊與導遊實務', credits: 2 },
          { type: '選修', name: '旅館資訊應用', credits: 2 },
          { type: '選修', name: '運動與健康（一）', credits: 2 },
          { type: '選修', name: '休閒應用英語（三）', credits: 2 },
          { type: '選修', name: '魅力商圈經營', credits: 3 },
          { type: '選修', name: '渡假村管理', credits: 3 },
          { type: '選修', name: '博弈管理', credits: 3 },
          { type: '選修', name: '俱樂部管理', credits: 3 },
          { type: '選修', name: '城市休閒資源開發規劃', credits: 3 },
          { type: '選修', name: '休閒農場管理', credits: 3 },
          { type: '選修', name: '遊程規劃與成本分析', credits: 3 },
          { type: '選修', name: '烹調實務', credits: 3 },
          { type: '選修', name: '文化產業與觀光', credits: 3 },
          { type: '選修', name: '運動與健康（二）', credits: 2 },
          { type: '選修', name: '校外實習輔導', credits: 2 },
          { type: '選修', name: '休閒應用英語（四）', credits: 2 },
          
        ],
        大四: [
          { type: '必修', name: '校外實習', credits: 9 },
          { type: '必修', name: '畢業專題', credits: 3 },
          { type: '必修', name: '休閒策略管理', credits: 3 },
          { type: '選修', name: '城市休閒投資財務規劃', credits: 3 },
          { type: '選修', name: '主題樂園管理與實務', credits: 3 },
          { type: '選修', name: '餐飲管理與實務', credits: 3 },
          { type: '選修', name: '休閒應用英語（五）', credits: 2 },

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
      studentCount: 0,
      teacherCount: 0,
      graduateCount: 0,
      phdCount: 0,
      isSidebarOpen: false,
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
    showInfoModal() {
      this.isInfoModalVisible = true;
    },
    closeInfoModal() {
      this.isInfoModalVisible = false;
    },
     showCreditModal(message) {
      console.log('showCreditModal called');
  this.creditMessage = `一、二年級一學期不能小於16學分，<br>三、四年級一學期不可以小於9學分，<br>全年級一學期不可以多餘25。<br><br>
  <span style="color: red;">畢業門檻</span><br>
  必修學分：98<br>
  選修總學分 (含本系/跨系選修)：24<br>
  博雅通識：4領域8學分<br>
  校定語言門檻：中級複試、多益550，或同等級數之英文能力檢定標準<br>
  `;
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
          data: [5, 4, 2],
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
    animateCount(target, endValue, duration) {
      let startValue = 0;
      let startTime = null;
      const step = (timestamp) => {
        if (!startTime) startTime = timestamp;
        const progress = timestamp - startTime;
        const currentValue = Math.min(
          Math.ceil((progress / duration) * endValue),
          endValue
        );
        this[target] = currentValue;
        if (progress < duration) {
          requestAnimationFrame(step);
        }
      };
      requestAnimationFrame(step);
    },
     toggleSidebar() {
      this.isSidebarOpen = !this.isSidebarOpen;
    },
    // 滾動到指定的區塊
    scrollToSection(sectionId) {
      const element = document.getElementById(sectionId);
      if (element) {
        element.scrollIntoView({ behavior: 'smooth' });
        this.toggleSidebar(); // 滾動後關閉側邊欄
      }
    },
  },
  mounted() {
    this.currentDetail = '課程';
    console.log('Current detail is:', this.currentDetail); 
    setTimeout(() => {
      this.isActive = true;
    });
    this.moveAndShowIntroduction(this.items[0]);
    this.animateCount('studentCount', 249, 2000); // 動畫持續 2 秒
    this.animateCount('teacherCount', 27, 2000);
    this.animateCount('graduateCount', 44, 2000);
    this.animateCount('phdCount', 22, 2000);
  },
  beforeUnmount() {
    if (this.chart) {
      this.chart.destroy();
    }
  },
  
};
</script>

<style scoped>
@import url('https://cdn.jsdelivr.net/npm/@flaticon/flaticon-uicons/css/all/all.css');
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 80%;
  height: 100vh;
  background-color: rgba(90, 90, 90, 0.326);
  display: flex;
  justify-content: center;  
  align-items: center;
  z-index: 9999; /* 確保彈窗位於最前面 */
}

.modal-content {
  width: 80%;
  background: white;
  padding: 20px;
  border-radius: 8px;
  z-index: 10000; /* 確保內容顯示在前 */
}
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
.highlight {
  background-color: yellow; /* 設置螢光筆顏色 */
  padding: 2px 4px; /* 增加內間距來模擬螢光筆的塗抹效果 */
  border-radius: 3px; /* 圓角處理，讓效果更自然 */
}
.item p {
  margin: 0;
  color: white;
  font-size: 16px;
  font-weight: bold;
}
.air{
  width: 100%;
  height: 80px;
  margin-top: 20%;
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
  height: 100%;
  overflow: auto;
}

.professor-list {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
  text-align: center;
}

.professor-card {
  width: 47%;
  justify-content: center;
  align-items: center;
  cursor: pointer;
  
}

.professor-image {
  width: 150px;
  height: 150px;
  border-radius: 50%;
}
.imin {
  width: 100%;
  height: auto;
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
.card4{
   background-color: #fff;
  border-radius: 20px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  overflow: hidden;
  width: 250px; /* 根據需要調整卡片寬度 */
  text-align: center;
  margin: 0 auto;
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
   position: relative;
  z-index: 1;
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
  margin-bottom: 5px;
  width: 40px; /* Adjusted size for better alignment */
  height: 40px;
  margin-right: 10px; 
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
  margin-top: -11%;
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
  margin-top: 0px;
  text-align: center;
}
.credits-summary div {
  margin-top: 5px;
  display: inline-flex; /* 保證圖標與文字在同一行排列 */
  align-items: center; /* 垂直居中對齊圖標與文字 */
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
  font-size: 15px;
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
  align-items: center;
 
}
.carousel-item {
  display: flex;
  justify-content: center;
  width: 100%;
  }


.card {
  background-color: #e7e0ff;
  border-radius: 20px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  margin-top: 20px;
  width: 100%; /* 根據需要調整卡片寬度 */
  text-align: center;
  
}
 .card3 {
  padding: 30px;
  background-color: #e7e0ff;
  border-radius: 20px;
  box-shadow: 0 8px 16px rgba(0, 0, 0, 0.1);
  text-align: left;
  max-width: 600px;
  margin-top: 20px;
}

.card3 h3 {
  text-align: center;
  font-size: 24px;
  margin-top: 0px;
  margin-bottom: 20px;
}

/* 研究所和就業工作欄 */
.graduate-section, .job-section {
  margin-bottom: 20px;
}

.graduate-section h4, .job-section h4 {
  font-size: 20px;
  color: #333;
  margin-bottom: 10px;
  font-weight: 700;
  text-align: left;
}

.graduate-section ul, .job-section ul {
  list-style: none;
  padding: 0;
  margin: 0;
}

.graduate-section ul li, .job-section ul li {
  font-size: 16px;
  line-height: 1.8;
  color: #555;
  padding-left: 20px;
  position: relative;
  margin-bottom: 10px;
}

.graduate-section ul li::before, .job-section ul li::before {
  content: '• '; /* 添加自定義圓點符號 */
  color: #527ba7;
  font-size: 30px;
  position: absolute;
  left: 0;  
  top: -13px;
}

.card2 {
  padding: 30px;
  background-color: #e7e0ff;
  border-radius: 20px;
  box-shadow: 0 8px 16px rgba(0, 0, 0, 0.1);
  text-align: left;
  max-width: 600px;
  margin-top: 20px;
}

.card2 h3 {
  font-size: 24px;
  color: #333;
  margin-bottom: 20px;
  text-align: center;
  font-weight: 700;
  margin-top: 0px;
}

.card2 ul {
  list-style: none; /* 移除預設的列表符號 */
  padding: 0;
}

.card2 ul li {
  font-size: 16px;
  line-height: 1.8;
  color: #555;
  padding-left: 20px;
  position: relative;
  margin-bottom: 10px;
}

.card2 ul li::before {
  content: '• '; /* 添加自定義圓點符號 */
  color: #527ba7;
  font-size: 30px;
  position: absolute;
  left: 0;  
  top: -13px;
}


h3 {
  color: #333;
  font-size: 24px;
}
.card1 {
  padding: 30px;
  background-color: #e7e0ff;
  border-radius: 20px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  max-width: 600px;
  margin: 0 auto;
  text-align: center;
}
.card1 h3{
  margin-top: 0px;
}
.stats-container {
  display: grid;
  grid-template-columns: 1fr 1fr; /* 兩列 */
  gap: 10px; /* 間距 */
}

.stat-item {
  background-color: white;
  border-radius: 10px;
  padding: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}
.stat-item h2,
.stat-item p {
  display: inline-block; /* 讓 h2 和 p 顯示在同一行 */
  margin: 0; /* 去除預設的 margin */
}
.stat-item p {
  font-size: 14px;
  margin: 0;
}

.stat-item h2 {
  color: red;
  margin: 5px 0;
  font-size: 24px;
}

.description {
  background-color: white;
  border-radius: 10px;
  padding: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  margin: 10px 0;
  font-size: 14px;
}

.imin {
  width: 100%;
  height: auto;
  margin-top: 10px;
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
.menu-icon {
  position: fixed;
  right: 26px;
  font-size: 30px;
  cursor: pointer;
  z-index: 1000;
}
@media (max-height: 668px) {
    .menu-icon {
    top: 14.5%;
  }
}

@media (min-height: 669px) and (max-height: 843px) {
  .menu-icon {
    top: 11.5%;
  }
}

@media (min-height: 844px) {
  .menu-icon {
    top: 11.5%;
  }
}
.sidebar {
  position: fixed;
  top: 0;
  right: -250px; /* 初始位置在屏幕外 */
  width: 220px;
  height: 38%;
  background-color: #cadbeaef;
  color: rgb(121, 143, 230);
  transition: right 0.3s ease-in-out;
  z-index: 999;
  margin-top: 100px;
  border-top-left-radius: 20px; /* 左上角圓角 */
  border-bottom-left-radius: 20px; 
}

.sidebar.is-open {
  right: 0; /* 當側邊欄打開時，移動到右邊 */
}

.sidebar .close-btn {
  text-align: right;
  padding: 10px;
  cursor: pointer;
}

.sidebar ul {
  list-style: none;
  padding: 0;
  margin-top: 25px;
}

.sidebar ul li {
  position: relative;
  padding: 15px 20px;
  cursor: pointer;
  font-size: 18px;
  font-weight: bold;
}

.sidebar ul li::after {
  content: '';
  position: absolute;
  left: 10%;
  right: 10%; /* 控制左右邊距，讓線條變短 */
  bottom: 0;
  height: 2px;
  background-color: #b4b4b4; /* 分隔線顏色 */
}

.custom-icon {
  cursor: pointer;
  z-index: 1000; /* 確保圖標位於頂層 */
  position: relative;/* 與左邊文字的距離 */
  display: inline-block;
  width: 24px; /* 確保有足夠的點擊區域 */
  height: 24px;
  cursor: pointer;
  font-size: 24px;
  margin-left: 80%;
}

/* 保證側邊欄和內容隨著滾動保持固定 */
.sidebar, .menu-icon {
  position: fixed;
}
.parent-container {
  overflow: visible; /* 確保子元素不會被裁剪 */
}
.custom-icon1 {
  margin-left: 10px;
  font-size: 18px;
  color: #080808;
  cursor: pointer;
  display: inline-block;
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
  width: 300px;
}
.close-icon {
  position: absolute;
  top: 10px;
  right: 10px;
  font-size: 24px;
  font-weight: bold;
  color: #333;
  cursor: pointer;
  transition: color 0.3s ease;
}
</style>
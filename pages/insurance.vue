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
        <h1>保險金融管理系</h1>
        <div v-if="currentDetail === '師資'">
          <!-- 圖表部分 -->
          <div class="chart-container card">
            <h2><br>師資</h2>
            <canvas id="teacherChart"></canvas>
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
          <p class="description">傳授紮實的保險及金融理論與知識，養成多面相學習習慣，將學生培育為「專業、敬業、樂業的保險與金融產業中高階層保險金融管理專才」。</p>
          

        </div>
      


      <!-- 發展計畫及未來展望區塊 -->
       <div id="發展計畫及未來展望" class="card2">
        <h3>發展計畫及未來展望</h3>
        <ul>
          <li>提升學生畢業後就業競爭力，課程規劃<span class="highlight">保險與金融理論</span>，著重學生<span class="highlight">實務之養成</span>，與保險公司、銀行等保險金融相關單位簽定<span class="highlight">產學合作</span>契約，提供學生<span class="highlight">實習</span>機會，落實<span class="highlight">理論與實務並重</span>。</li>
          <li>與<span class="highlight">壽險公會</span>合辦保險相關證照考試，並與在<span class="highlight">校內舉辦</span>之金融研訓院各項金融證照，訂有<span class="highlight">「國立臺中科技大學學生取得專業技術證照獎勵要點」</span>及<span class="highlight">「商學院學生取得專業證照獎勵要點」</span>。</li>
          <li>鼓勵教師積極參與研究、提升教學品質，訂有<span class="highlight">「國立臺中科技大學保險金融管理系教師學術研究獎勵補助要點」</span>，每年爭取<span class="highlight">科技部社會科學中心</span>到本系舉辦學術性講座與交流。</li>
          <li>本系具悠久歷史，畢業校友在金融與保險業素有口碑，透過本系教學活動與<span class="highlight">系友會</span>之連結，以及系友回娘家活動，有益學生在畢業之際，即可獲得廣大之<span class="highlight">人脈資源</span>。</li>
          <li>設有<span class="highlight">「保險金融服務中心」</span>，由保險金融專業經理人到校提供業界<span class="highlight">免費諮詢</span>，並舉辦多次專業證照檢定，配合諮詢心得競賽，以鼓勵各學制學生藉由課後諮詢，領略<span class="highlight">經理人實務經驗</span>、並提升學生<span class="highlight">溝通簡報</span>能力。</li>
          <li>組成<span class="highlight">保金志工隊</span>，除了常駐於保金中心服務外，並至<span class="highlight">中小學</span>辦理理財教育活動，讓參與學子在遊戲中輕鬆學習理財與投資。</li>       
        </ul>
      </div>

      <!-- 畢業區塊 -->
        <div id="畢業" class="card3">
          <h3>發展方向</h3>
          
          <!-- 研究所一欄 -->
        

          <!-- 就業工作一欄 -->
          <div class="job-section">
            <h4>共同出路</h4>
            <ul>
              <li>一般企業</li>
              <li>行政人員</li>
              <li>會計/出納人員</li>
              <li>財務人員</li>
              <li>客戶服務人員</li>
              <li>總務人員</li>              
            </ul>
             <h4>銀行相關</h4>
            <ul>
              <li>理財投資/規劃人員</li>
              <li>金融營業員</li>
              <li>銀行辦事員</li>
              <li>銀行外匯辦事員</li>
              <li>基金經理人員</li>
            </ul>
             <h4>保險相關</h4>
            <ul>
              <li>保險業務員</li>
              <li>壽險/產險核保理賠人員</li>
              <li>人身/財產保險代理人</li>
              <li>人身/財產保險經紀人</li>
              <li>個人/企業/金融風險管理師</li>
              <li>人壽保險管理人員</li>
              <li>中華民國精算師</li>              
            </ul>
             <h4>證券期貨相關</h4>
            <ul>
              <li>期貨營業員</li>
              <li>證券分析師/營業員</li>
              <li>證券/期貨經紀人</li>
              <li>CFP個人理財規劃師</li>
              <li>CFA企業理財規劃師</li>              
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
            <button @click="closeCreditModal">關閉</button>
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
        this.animateCount('studentCount', 346, 2000); // 動畫持續 2 秒
        this.animateCount('teacherCount', 26, 2000);
        this.animateCount('graduateCount', 73, 2000);
        this.animateCount('phdCount', 22, 2000);
      }
    },
  },
  el: '#app',
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
          { name: '姜老師', image: '/Images/teacher2.png', expertise: '資料探勘、數據分析、統計與計量方法、派翠網路、生物醫療、訊號處理' },
          { name: '黃老師', image: '/Images/teacher.png', expertise: '數位學習、虛擬實境與擴增實境、智慧型資訊系統、電子商務' },
          { name: '陳老師', image: '/Images/teacher.png', expertise: '網路協定與演算法、物聯網、功耗感知、即時系統、網際網路應用、無線網路、雲端運算' },
          { name: '蕭老師', image: '/Images/teacher2.png', expertise: '電子商務、網路行銷、電子化企業、知識管理、科技管理、管理決策' },
          { name: '姜老師', image: '/Images/teacher2.png', expertise: '資料探勘、數據分析、統計與計量方法、派翠網路、生物醫療、訊號處理' },
          { name: '黃老師', image: '/Images/teacher.png', expertise: '數位學習、虛擬實境與擴增實境、智慧型資訊系統、電子商務' },
          
        ],
        副教授: [
          { name: '王老師', image: '/Images/teacher.png', expertise: '行動加值與應用、智慧型人機介面、雲端與網路服務系統、多變量分析、企業電子化系統' },
          { name: '廖老師', image: '/Images/teacher2.png', expertise: '資訊視覺化、電腦視覺、計算機圖學' },
          { name: '駱老師', image: '/Images/teacher2.png', expertise: '網路安全、資訊網路、網路應用、電子商務安全、醫療資訊安全' },
          { name: '張老師', image: '/Images/teacher.png', expertise: '人力資源、職涯規劃、行銷管理' },
          { name: '何老師', image: '/Images/teacher2.png', expertise: '專長及研究領域：組織行為、領導、建言行為、人力資源管理、動機取向' },
          { name: '林老師', image: '/Images/teacher.png', expertise: '元宇宙學習應用、多媒體應用與開發、數位學習與應用、雲端系統應用與虛擬化、網路安全、行動通訊、計算機網路、智慧物聯網、AI教學應用' },
          { name: '侯老師', image: '/Images/teacher.png', expertise: '數位學習與大數據分析、程式設計與應用、統計學、多變量分析' },
          { name: '王老師', image: '/Images/teacher.png', expertise: '行動加值與應用、智慧型人機介面、雲端與網路服務系統、多變量分析、企業電子化系統' },
          { name: '廖老師', image: '/Images/teacher2.png', expertise: '資訊視覺化、電腦視覺、計算機圖學' },
          { name: '何老師', image: '/Images/teacher2.png', expertise: '專長及研究領域：組織行為、領導、建言行為、人力資源管理、動機取向' },
          { name: '林老師', image: '/Images/teacher.png', expertise: '元宇宙學習應用、多媒體應用與開發、數位學習與應用、雲端系統應用與虛擬化、網路安全、行動通訊、計算機網路、智慧物聯網、AI教學應用' },
          { name: '侯老師', image: '/Images/teacher.png', expertise: '數位學習與大數據分析、程式設計與應用、統計學、多變量分析' },
          { name: '王老師', image: '/Images/teacher.png', expertise: '行動加值與應用、智慧型人機介面、雲端與網路服務系統、多變量分析、企業電子化系統' },
          { name: '廖老師', image: '/Images/teacher2.png', expertise: '資訊視覺化、電腦視覺、計算機圖學' },          
        ],
        助理教授: [
          { name: '羅老師', image: '/Images/teacher.png', expertise: '科技教育、資訊管理' },
        ]
      },
      selectedYear: '請選擇年級', // Default selected year
      isDropdownOpen: false, // Control the dropdown state
      years: ['大一', '大二', '大三', '大四'], // Dropdown options
      currentCourses: [], // List of courses for the selected year
      courses: {
        大一: [
          { type: '必修', name: '經濟學', credits: 3 },
          { type: '必修', name: '會計學	', credits: 3 },
          { type: '必修', name: '英文', credits: 3 },
          { type: '必修', name: '微積分', credits: 2 },
          { type: '必修', name: '風險管理與保險	', credits: 2 },
          { type: '必修', name: '國文', credits: 2 },
          { type: '必修', name: '體育', credits: 0 },
          { type: '必修', name: '程式設計與應用	', credits: 2 },
          { type: '必修', name: '大學之道', credits: 2 },
          { type: '必修', name: '服務與學習', credits: 1 },
          { type: '必修', name: '學涯規劃	', credits: 1 },
          { type: '選修', name: '貨幣銀行學', credits: 2 },
          { type: '選修', name: '英語檢定', credits: 1 },
          { type: '選修', name: '民法', credits: 3 },
          { type: '選修', name: '資料處理', credits: 2 },
          { type: '選修', name: '工業安全與衛生', credits: 2 },
          { type: '選修', name: '商事法', credits: 3 },
          { type: '選修', name: '組織理論與管理', credits: 2 },
          { type: '選修', name: '資料蒐集與計畫書寫作', credits: 1 },
          { type: '選修', name: '商業套裝軟體', credits: 1 },
        ],
        大二: [
          { type: '必修', name: '統計學', credits: 3 },
          { type: '必修', name: '財務管理', credits: 2 },
          { type: '必修', name: '生涯運動', credits: 0 },
          { type: '必修', name: '財產保險', credits: 3 },
          { type: '必修', name: '風險管理策略', credits: 3 },
          { type: '必修', name: '保險法規', credits: 3 },
          { type: '必修', name: '臺灣開發史', credits: 2 },
          { type: '必修', name: '金融法規', credits: 3 },
          { type: '必修', name: '人身保險', credits: 3 },
          { type: '必修', name: '民主憲政與法治', credits: 2 },
          { type: '選修', name: '英語檢定（二）', credits: 1 },
          { type: '選修', name: '個體經濟學', credits: 3 },
          { type: '選修', name: '保險行銷', credits: 3 },
          { type: '選修', name: '金融行銷', credits: 3 },
          { type: '選修', name: '金融市場', credits: 3 },
          { type: '選修', name: '中級會計學', credits: 3 },
          { type: '選修', name: '顧客關係管理', credits: 3 },
          { type: '選修', name: '總體經濟學', credits: 3 },
          { type: '選修', name: '理財規劃', credits: 3 },
          { type: '選修', name: '國際金融與匯兌', credits: 3 },
        ],
        大三: [
          { type: '必修', name: '投資學', credits: 3 },
          { type: '必修', name: '利率學', credits: 3 },
          { type: '必修', name: '應用統計', credits: 2 },
          { type: '必修', name: '保險金融資訊系統（一）', credits: 2 },
          { type: '必修', name: '財務報表分析', credits: 3 },
          { type: '必修', name: '金融機構管理', credits: 3 },
          { type: '必修', name: '保險金融資訊系統（二）', credits: 2 },
          { type: '必修', name: '應用文與習作', credits: 2 },
          { type: '必修', name: '職涯規劃', credits: 1 },
          { type: '選修', name: '日文（一）', credits: 2 },
          { type: '選修', name: '校外銀行實務', credits: 3 },
          { type: '選修', name: '期貨與選擇權', credits: 3 },
          { type: '選修', name: '財產保險核保理賠', credits: 3 },
          { type: '選修', name: '證券法規與實務	', credits: 2 },
          { type: '選修', name: '社會福利與保險規劃', credits: 3 },
          { type: '選修', name: '不動產估價理論', credits: 3 },
          { type: '選修', name: '財產風險管理', credits: 2 },
          { type: '選修', name: '保險數學', credits: 2 },
          { type: '選修', name: '保險會計', credits: 2 },
          { type: '選修', name: '信用風險衡量與管理', credits: 2 },
          { type: '選修', name: '金融科技', credits: 2 },
          { type: '選修', name: '自主性學習（一）', credits: 1 },
          { type: '選修', name: '授信理論與實務', credits: 3 },
          { type: '選修', name: '風險管理數學', credits: 3 },
          { type: '選修', name: '保險專門職業人員證照輔導（一）', credits: 3 },
          { type: '選修', name: '金融專業證照輔導（一）', credits: 3 },
          { type: '選修', name: '投資組合與管理', credits: 3 },
          { type: '選修', name: '人身保險核保理賠', credits: 3 },
          { type: '選修', name: '專題製作（一）', credits: 2 },
          { type: '選修', name: '國際禮儀與職場倫理', credits: 2 },
          { type: '選修', name: '再保險', credits: 2 },
          { type: '選修', name: '市場風險衡量與管理', credits: 2 },
          { type: '選修', name: '巨量資料概論', credits: 2 },
          { type: '選修', name: '人身風險管理', credits: 2 },
        ],
        大四: [
          { type: '必修', name: '財產保險實務', credits: 3 },
          { type: '必修', name: '信託投資與實務', credits: 3 },
          { type: '必修', name: '人身保險實務', credits: 3 },
          { type: '選修', name: '日文（二）', credits: 2 },
          { type: '選修', name: '電子商務', credits: 3 },
          { type: '選修', name: '保險專門職業人員證照輔導（二）', credits: 3 },
          { type: '選修', name: '金融專業證照輔導（二）', credits: 3 },
          { type: '選修', name: '不動產經濟學', credits: 3 },
          { type: '選修', name: '不動產投資與管理', credits: 3 },
          { type: '選修', name: '專題製作（二）', credits: 2 },
          { type: '選修', name: '商業英文雜誌選讀', credits: 2 },
          { type: '選修', name: '財產保險經營', credits: 2 },
          { type: '選修', name: '信託法規與實務', credits: 2 },
          { type: '選修', name: '永續經營與綠色金融保險', credits: 2 },
          { type: '選修', name: '校外實習（一）', credits: 2 },
          { type: '選修', name: '問題導向', credits: 1 },
          { type: '選修', name: '校外實習（二）', credits: 9 },
          { type: '選修', name: '資產證券化理論與實務', credits: 3 },
          { type: '選修', name: '債券市場理論與實務', credits: 3 },
          { type: '選修', name: '租稅規劃與實務', credits: 3 },
          { type: '選修', name: '經營與整合性風險管理', credits: 2 },
          { type: '選修', name: '國際經濟現勢', credits: 2 },
          { type: '選修', name: '國際保險市場', credits: 2 },
          { type: '選修', name: '員工福利與退休金規劃', credits: 2 },
          { type: '選修', name: '危機管理', credits: 2 },
          { type: '選修', name: '人身保險經營', credits: 2 },

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
     showCreditModal(message) {
      console.log('showCreditModal called');
  this.creditMessage = `一、二年級一學期不能小於16學分，<br>三、四年級一學期不可以小於9學分，<br>全年級一學期不可以多餘25。<br><br>
  <span style="color: red;">畢業門檻</span><br>
  必修學分：95<br>
  選修總學分 (含本系/跨系選修)：40<br>
  博雅通識：4領域8學分<br>
  校定語言門檻：中級複試、多益550，或同等級數之英文能力檢定標準<br>
  <span style="color: red;">系定門檻：</span><br>
  1.專題製作（一）與專題製作（二）或校外實習（一），須擇一作為畢業門檻<br>
  2.專業證照累積點數達 5 點(含)以上`;
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
          data: [6, 14, 1],
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
  width: 45%;
  justify-content: center;
  align-items: center;
  cursor: pointer;
}

.professor-image {
  width: 100%;
  height: auto;
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
</style>
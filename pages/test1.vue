<template>
  <div class="page-container">
    <!-- 學院選單 -->
     <div v-if="!isCollegeSelected" class="college-selector">
      <h2>請選擇學院</h2>
      <div class="college-grid">
        <div 
          v-for="(tab, index) in tabs" 
          :key="index" 
          :class="'college-item item-' + (index + 1)"
          @click="selectCollege(tab.type)"
        >
          <i :class="tab.icon" class="college-icon"></i>
          <p>{{ tab.name }}</p>
        </div>
      </div>
    </div>

    <!-- 學院內容 -->
    <div v-else>
      <div class="tabs">
        <button
          v-for="(tab, index) in tabs"
          :key="index"
          :data-type="tab.type" 
          @click="changeTab(tab.type)"
          :class="{ active: currentTab === tab.type }"
        >
          <i :class="tab.icon"></i>
        </button>
      </div>
      <transition name="fade" mode="out-in">
        <div :key="currentTab" class="card-container">
          <!-- 資訊與流通學院 -->
          <div v-if="currentTab === 'info'" class="about-section">
            <div class="image-container">
              <img src="/Images/im.png" alt="Image" />
            </div>
            <div class="text-container">
              <p>
                落實創新服務、培育立型人才。<br />
                培育不同層面之務實致用人才，滿足產業界的需求。
              </p>
              <div class="dropdown-list" v-show="showDropdown">
                <transition-group name="list" tag="div">
                  <div
                    v-for="(item, index) in infoItems"
                    :key="index"
                    class="dropdown-item"
                    :class="{ 'active-blue': activeTab === item.link || (currentTab === 'teeest' && activeTab === item.link) }"
                    :style="{ animationDelay: `${index * 0.5}s`, backgroundColor: item.color }"
                    @click="changeTab(item.link, 'info')"
                  >
                    {{ item.text }}
                  </div>
                </transition-group>
              </div>
            </div>
          </div>

          <!-- 商學院 -->
          <div v-else-if="currentTab === 'business'" class="about-section">
            <div class="image-container">
              <img src="/Images/business.png" alt="Image" />
            </div>
            <div class="text-container">
              <p>
                專業創新之商業跨域人才培育基地。<br />
                培育專業創新與敬業當責之商業管理人才。
              </p>
              <div class="dropdown-list" v-show="showDropdown">
                <transition-group name="list" tag="div">
                  <div
                    v-for="(item, index) in businessItems"
                    :key="index"
                    class="dropdown-item"
                    :style="{ animationDelay: `${index * 0.5}s`, backgroundColor: item.color }"
                    @click="changeTab(item.link, 'business')"
                  >
                    {{ item.text }}
                  </div>
                </transition-group>
              </div>
            </div>
          </div>

          <!-- 設計學院 -->
          <div v-else-if="currentTab === 'design'" class="about-section">
            <div class="image-container">
              <img src="/Images/design.png" alt="Image" />
            </div>
            <div class="text-container">
              <p>
                培育跨領域整合設計人才，成為台灣中部文創與關懷設計中心。<br />
                主動積極與追求績效。
              </p>
              <div class="dropdown-list" v-show="showDropdown">
                <transition-group name="list" tag="div">
                  <div
                    v-for="(item, index) in designItems"
                    :key="index"
                    class="dropdown-item"
                    :style="{ animationDelay: `${index * 0.5}s`, backgroundColor: item.color }"
                    @click="changeTab(item.link, 'design')"
                  >
                    {{ item.text }}
                  </div>
                </transition-group>
              </div>
            </div>
          </div>

          <!-- 智慧產業學院 -->
          <div v-else-if="currentTab === 'smart'" class="about-section">
            <div class="image-container">
              <img src="/Images/intelligence.png" alt="Image" />
            </div>
            <div class="text-container">
              <p>
                朝向智慧產業技術跨領域永續發展。<br />
                發展智慧商業、智慧生產與智慧醫療三大領域。
              </p>
              <div class="dropdown-list" v-show="showDropdown">
                <transition-group name="list" tag="div">
                  <div
                    v-for="(item, index) in smartItems"
                    :key="index"
                    class="dropdown-item"
                    :style="{ animationDelay: `${index * 0.5}s`, backgroundColor: item.color }"
                    @click="changeTab(item.link, 'smart')"
                  >
                    {{ item.text }}
                  </div>
                </transition-group>
              </div>
            </div>
          </div>

          <!-- 語文學院 -->
          <div v-else-if="currentTab === 'language'" class="about-section">
            <div class="image-container">
              <img src="/Images/languages.png" alt="Image" />
            </div>
            <div class="text-container">
              <p>
                培養中、英、日語文應用及數位商務人才。<br />
                紮根本土並接軌國際，提升學生文字、數位、外語、商業應用等能力。
              </p>
              <div class="dropdown-list" v-show="showDropdown">
                <transition-group name="list" tag="div">
                  <div
                    v-for="(item, index) in languageItems"
                    :key="index"
                    class="dropdown-item"
                    :style="{ animationDelay: `${index * 0.5}s`, backgroundColor: item.color }"
                    @click="changeTab(item.link, 'language')"
                  >
                    {{ item.text }}
                  </div>
                </transition-group>
              </div>
            </div>
          </div>

          <!-- 其他內容 -->
          <div v-else>
            <teeest :course="currentTab" @back="changeTab('info')" />
          </div>
        </div>
      </transition>
    </div>
  </div>
</template>

<script>
import teeest from './teeest.vue';

export default {
  name: 'MainPage',
  components: {
    teeest,
  },
  data() {
    return {
      currentTab: 'info',
      activeTab: 'info',
      showDropdown: true,
      isCollegeSelected: false,
      selectedCollege: '',
      tabs: [
        { name: '資訊與流通學院', type: 'info', icon: 'fi fi-bs-computer' },
        { name: '商學院', type: 'business', icon: 'fi fi-bs-corporate' },
        { name: '設計學院', type: 'design', icon: 'fi fi-br-paintbrush-pencil' },
        { name: '智慧產業學院', type: 'smart', icon: 'fi fi-rs-microchip-ai' },
        { name: '語文學院', type: 'language', icon: 'fi fi-rr-book-font' }
      ],
      infoItems: [
        { text: '資訊管理系', link: 'teeest', color: '#F4C953' },
        { text: '資訊工程系', link: 'cs', color: '#942B30' },
        { text: '流通管理系', link: 'logistics', color: '#F4C953' }
      ],
      businessItems: [
        { text: '國際貿易經營系', link: 'international', color: '#617E6C' },
        { text: '會計資訊系', link: 'accounting', color: '#77871B' },
        { text: '保險金融管理系', link: 'insurance', color: '#C56D59' },
        { text: '財政稅務系', link: 'tax', color: '#EEC3B4' },
        { text: '財務金融系', link: 'finance', color: '#617E6C' },
        { text: '休閒事業經營系', link: 'leisure', color: '#77871B' },
        { text: '應用統計系', link: 'statistics', color: '#C56D59' }
      ],
      designItems: [
        { text: '商業設計系', link: 'business', color: '#21A633' },
        { text: '多媒體設計系', link: 'multimedia', color: '#309EC8' },
        { text: '室內設計系', link: 'interior', color: '#FD6106' },
        { text: '創意商品服務系', link: 'creative', color: '#66d2a8' }
      ],
      smartItems: [
        { text: '商業經營系', link: 'business-management', color: '#EDBB8F' },
        { text: '智慧生產工程系', link: 'smart-production', color: '#38E5BB' }
      ],
      languageItems: [
        { text: '應用英文系', link: 'english', color: '#BEB8B2' },
        { text: '應用日語系', link: 'japanese', color: '#F89C63' },
        { text: '應用中文系', link: 'chinese', color: '#447970' }
      ]
    };
  },
  methods: {
    changeTab(tabType, activeTabType = tabType) {
      this.showDropdown = false;
      setTimeout(() => {
        this.currentTab = tabType;
        if (tabType === 'teeest') {
          this.activeTab = 'info';
        } else {
          this.activeTab = activeTabType;
        }
        this.showDropdown = true;
      }, 300);
    },
    selectCollege(tabType) {
      this.currentTab = tabType;
      this.isCollegeSelected = true;
    }
  },
  mounted() {
    this.$nextTick(() => {
      const buttons = document.querySelectorAll('.tabs button');
      buttons.forEach(button => {
        button.classList.remove('active');
      });
      const infoButton = document.querySelector(`.tabs button[data-type="info"]`);
      if (infoButton) {
        infoButton.classList.add('active');
      }
    });
  }
}
</script>

<style>
@import url('https://cdn.jsdelivr.net/npm/@flaticon/flaticon-uicons/css/all/all.css');
/* 新增的選單樣式 */
.college-selector {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  min-height: 100vh;
}

.college-grid {
  position: relative;
  width: 200px;
  height: 250px;
}

.college-item {
  position: absolute;
  width: 100px;
  height: 100px;
  display: flex;
  justify-content: center;
  align-items: center;
  transform: rotate(45deg);
  cursor: pointer;
}

.diamond {
  width: 100%;
  height: 100%;
  background-color: #1E90FF;
  display: flex;
  justify-content: center;
  align-items: center;
  transform: rotate(0deg);
  transition: transform 0.3s ease, background-color 0.3s ease;
}

.college-item:hover .diamond {
  transform: scale(1.1) rotate(0deg);
}

.college-icon, .college-item p {
  color: white;
  text-align: center;
}

.college-icon {
  font-size: 2rem;
}

.college-item p {
  margin: 0;
  font-size: 1rem;
}

/* 菱形颜色和位置调整 */
.item-1 {
  background-color: #1E90FF;
  top: 0;
  left: 50%;
  transform: translate(-50%, -50%) rotate(45deg);
}

.item-2 {
  background-color: #00BFFF;
  top: 50%;
  left: 100%;
  transform: translate(-50%, -50%) rotate(45deg);
}

.item-3 {
  background-color: #FFD700;
  top: 100%;
  left: 50%;
  transform: translate(-50%, -50%) rotate(45deg);
}

.item-4 {
  background-color: #FF6347;
  top: 50%;
  left: 0;
  transform: translate(-50%, -50%) rotate(45deg);
}

.item-5 {
  background-color: #8A2BE2;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%) rotate(45deg);
}



/* 現有的樣式保持不變 */
.active-blue {
  background-color: rgb(24, 66, 144) !important; /* 蓝色 */
  color: white !important; /* 白色字体 */
}
body {
  background-color: white;
  color: black;
  margin: 0;
  padding: 0;
  font-family: Arial, sans-serif;
  display: flex;
  justify-content: flex-start;
  align-items: center;
  min-height: 100vh;
  flex-direction: column;
}
.combined-enter-active, .combined-leave-active {
  transition: all 0.5s cubic-bezier(0.25, 0.8, 0.5, 1);
}

.combined-enter, .combined-leave-to {
  opacity: 0;
  transform: translateX(15px) scale(0.98);
}

.tabs {
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  justify-content: center;
  margin-bottom: 0;
  position: relative;
  width: 100%;
  top: 0;
}

.tabs button {
  background: none;
  border: none;
  color: black;
  font-size: 1.2rem;
  margin: 0;
  padding: 0.75rem 1.5rem;
  cursor: pointer;
  transition: background 0.3s;
  border-top-left-radius: 10px;
  border-top-right-radius: 10px;
  z-index: 1;
  position: relative;
  width: 100%; /* 確保按鈕填滿容器 */
}

.tabs button.active {
  background: rgb(24, 66, 144) !important;
  color: white !important;
}

.tabs button:not(.active) {
  background: rgba(24, 66, 144, 0.1) !important;
  color: black !important;
}

.tabs button:hover {
  background: rgb(24, 66, 144); /* 鼠标悬停时应用背景颜色 */
  color: white; /* 鼠标悬停时应用字体颜色 */
}

.page-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
  min-height: 100vh; /* 確保高度覆盖整个视窗 */
}

.card-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
  min-height: 100vh; /* 背景覆盖整个视窗高度 */
  background-image: url('/Images/bg.png');
  background-size: cover; /* 背景图片覆盖整个容器 */
  background-position: center; /* 背景图片居中 */
  background-repeat: no-repeat; /* 防止背景图片重复 */
}

.about-section {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: flex-start;
  color: white;
  padding: 2rem;
  width: 100%; /* 確保宽度填满容器 */
  margin: 0;
  min-height: calc(100vh - 80px);
}

.image-container {
  width: 100%;
  text-align: center;
  margin-bottom: 1rem;
}

.image-container img {
  border-radius: 10%/50%;
  width: 100%;
  height: auto;
  max-width: 400px;
}

.text-container {
  text-align: center;
  margin-bottom: 2rem;
  width: 100%;
  position: relative;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.text-container h1 {
  font-size: 2.5rem;
  margin: 0;
}

.text-container p {
  font-size: 1.2rem;
  margin-top: 1rem;
  padding: 0 1rem;
  text-align: center;
  transition: background-color 0.3s;
  border-left: none;
}

.text-container p:hover {
  background-color: rgba(38, 44, 101, 0.1);
}

.triangle {
  position: absolute;
  left: 10px;
  top: 50%;
  transform: translateY(-50%);
  width: 0;
  height: 0;
  border-left: 10px solid transparent;
  border-right: 10px solid transparent;
  border-top: 10px solid rgb(38, 44, 101);
}

.dropdown-list {
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.dropdown-item {
  width: 90%;
  max-width: 400px;
  text-align: center;
  margin-top: 1rem;
  padding: 0.5rem 1rem;
  font-size: 1.2rem;
  border-radius: 5px;
  transition: background-color 0.3s, transform 0.3s;
  cursor: pointer;
  display: flex;
  justify-content: center;
  align-items: center;
  white-space: nowrap;
}

.dropdown-item:hover {
  background-color: rgba(195, 198, 221, 1);
}

@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(-10px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.modal-enter-active,
.modal-leave-active {
  transition: opacity 0.3s;
}

.modal-enter,
.modal-leave-to {
  opacity: 0;
}

@media (max-width: 600px) {
  .tabs button {
    font-size: 1rem;
    padding: 0.5rem 1rem;
  }

  .about-section {
    padding: 1rem;
    min-height: calc(100vh - 80px);
  }

  .text-container h1 {
    font-size: 2rem;
  }

  .text-container p {
    font-size: 1rem;
  }

  .dropdown-item {
    font-size: 1rem;
    width: 90%;
  }
}
</style>

<template>
  <div class="page-container">
    <!-- 學院選單 -->
    <div v-if="!isCollegeSelected" class="college-selector">
      <h2>學院介紹</h2>
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
          :style="{ backgroundColor: currentTab === tab.type ? tab.color : '' }"
        >
          <i :class="tab.icon"></i>
        </button>
      </div>
      <transition name="fade" mode="out-in">
        <div :key="currentTab" class="card-container" :style="{ backgroundColor: backgroundColor }">
          <button class="back-button" @click="goBack">返回</button>
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
            <teeest v-if="currentTab === 'teeest'" :course="currentTab" @back="changeTab('info')" />
          </div>
        </div>
      </transition>
    </div>
  </div>
</template>

<script>
import teeest from './teeest.vue';
export default {
  name: 'Deintroduce',
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
      backgroundColor: '',
      tabs: [
        { name: '資訊與流通學院', type: 'info', icon: 'fi fi-bs-computer', color: '#83C3DA' },
        { name: '商學院', type: 'business', icon: 'fi fi-bs-corporate', color: '#8A95CA' },
        { name: '設計學院', type: 'design', icon: 'fi fi-br-paintbrush-pencil', color: '#B69DD5' },
        { name: '智慧產業學院', type: 'smart', icon: 'fi fi-rs-microchip-ai', color: '#9FB6C6' },
        { name: '語文學院', type: 'language', icon: 'fi fi-rr-book-font', color: '#3497CC' }
      ],
      infoItems: [
        { text: '資訊管理系', link: 'teeest', color: ' #5A7D8C ' },
        { text: '資訊工程系', link: 'cs', color: ' #5A7D8C ' },
        { text: '流通管理系', link: 'logistics', color: ' #5A7D8C ' }
      ],
      businessItems: [
        { text: '國際貿易經營系', link: 'international', color: '#355664' },
        { text: '會計資訊系', link: 'accounting', color: '#355664' },
        { text: '保險金融管理系', link: 'insurance', color: '#355664' },
        { text: '財政稅務系', link: 'tax', color: '#355664' },
        { text: '財務金融系', link: 'finance', color: '#355664' },
        { text: '休閒事業經營系', link: 'leisure', color: '#355664' },
        { text: '應用統計系', link: 'statistics', color: '#355664' }
      ],
      designItems: [
        { text: '商業設計系', link: 'business', color: '#5A423B' },
        { text: '多媒體設計系', link: 'multimedia', color: '#5A423B' },
        { text: '室內設計系', link: 'interior', color: '#5A423B' },
        { text: '創意商品服務系', link: 'creative', color: '#5A423B' }
      ],
      smartItems: [
        { text: '商業經營系', link: 'business-management', color: '#3B5C5B' },
        { text: '智慧生產工程系', link: 'smart-production', color: '#3B5C5B' }
      ],
      languageItems: [
        { text: '應用英文系', link: 'english', color: '#2F4F4F' },
        { text: '應用日語系', link: 'japanese', color: '#2F4F4F' },
        { text: '應用中文系', link: 'chinese', color: '#2F4F4F' }
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
        const selectedTab = this.tabs.find(tab => tab.type === tabType);
        if (selectedTab) {
          this.backgroundColor = selectedTab.color; 
          document.documentElement.style.setProperty('--button-color', selectedTab.color); 
        } else {
          console.error(`Tab type ${tabType} not found in tabs array.`);
        }
        this.$router.push({ query: { college: this.activeTab, dept: tabType } });
        this.showDropdown = true;
      }, 300);
    },
    selectCollege(tabType) {
      this.currentTab = tabType;
      this.isCollegeSelected = true;
      const selectedTab = this.tabs.find(tab => tab.type === tabType);
      this.backgroundColor = selectedTab.color; 
      document.documentElement.style.setProperty('--button-color', selectedTab.color);
    },
    selectCollegeAndDept(college, dept) {
      const selectedCollege = this.tabs.find(tab => tab.type === college);
      if (selectedCollege) {
        this.selectCollege(college);
        if (college === 'info') {
          const deptItem = this.infoItems.find(item => item.link === dept);
          if (deptItem) {
            this.changeTab(deptItem.link, 'info');
          } else {
            console.error(`Department ${dept} not found in infoItems.`);
          }
        }
        // 你可以在這裡添加其他學院的條件判斷，例如:
        // else if (college === 'business') { ... }
      } else {
        console.error(`College type ${college} not found.`);
      }
    },
    goBack() {
      const mainTabs = ['info', 'business', 'design', 'smart', 'language']; 

      if (!mainTabs.includes(this.currentTab)) {
        this.changeTab(this.activeTab); 
      } else {
        this.isCollegeSelected = false;
      }
    }
  },
  mounted() {
    this.$nextTick(() => {
        // 取得URL中的參數
        const queryCollege = this.$route.query.college;
        const queryDept = this.$route.query.dept;

        if (queryCollege && queryDept) {
            this.selectCollegeAndDept(queryCollege, queryDept);
        } else {
            setTimeout(() => {
                const infoButton = document.querySelector(`.tabs button[data-type="info"]`);
                if (infoButton) {
                    infoButton.click(); // 手动触发点击事件
                    infoButton.classList.add('active'); // 确保它被设置为激活状态
                }
            }, 300); // 延迟 300 毫秒以确保页面完全加载
        }

        // 手动设置激活状态
        if (queryDept && this.currentTab === queryDept) {
            const activeButton = document.querySelector(`.tabs button[data-type="${queryDept}"]`);
            if (activeButton) {
                activeButton.classList.add('active');
            }
        }
    });
}
}
</script>



<style>
@import url('https://cdn.jsdelivr.net/npm/@flaticon/flaticon-uicons/css/all/all.css');
.back-button {
  position: absolute;
  top: 70%;
  left: 5%;
  background-color: #6e73dc;
  color: #ffffff;
  border: none;
  padding: 8px 16px;
  font-size: 14px;
  border-radius: 4px;
  cursor: pointer;
  z-index: 1000;
}
/* 新增的選單樣式 */
.college-selector h2 {
  margin-top: 55%;
  margin-bottom: 100px; /* 增加上方的空间，防止被挡住 */
  z-index: 10; /* 确保标题位于菱形之上 */
}
.college-selector {
   display: flex;
  flex-direction: column;
  align-items: center;
}


.college-grid {
  position: relative;
  width: 200px;
  height: 250px;
}

.college-item {
  position: absolute;
  width: 120px;
  width: 120px;
  height: 120px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  transform: rotate(45deg);
  cursor: pointer;
  text-align: center;
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
  .college-item .college-icon {
    font-size: 2rem; 
  }

  .college-icon, .college-item p {
    margin: 0;
    font-size: 1rem;
    transform: rotate(-45deg);
    text-align: center;
    color: white;
  
  }

  .college-icon {
    font-size: 2rem;
    margin-left: -30px;
  }

  .college-item p {
  margin: 0;
    font-size: 1.1rem; /* 文字大小 */
    line-height: 1.2; 
    transform: rotate(-45deg); 
    text-align: center;
    margin-top: -10px;
    margin-left: 40px;
  }
.college-item .content-container {
  transform: rotate(-45deg); 
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
/* 菱形颜色和位置调整 */
.item-1 {
  background-color: #83C3DA;
  top: 15%;
  left: 25%;
  transform: translate(-50%, -50%) rotate(45deg);
}

.item-2 {
  background-color: #8A95CA;
  top: 113%;
  left: -2%;
  transform: translate(-50%, -50%) rotate(45deg);
}
.item-2 .college-icon {
  margin-left: 5px; 
}

.item-3 {
  background-color: #B69DD5;
  top: 75%;
  left: 50%;
  transform: translate(-50%, -50%) rotate(45deg);
}
.item-3 .college-icon {
  margin-left: -8px; 
}
.item-4 {
  background-color: #9FB6C6;
  top: 37%;
  left: 100%;
  transform: translate(-50%, -50%) rotate(45deg);
}

.item-5 {
  background-color: #3497CC;
  top: 133%;
  left: 75%;
  transform: translate(-50%, -50%) rotate(45deg);
}
.item-5 .college-icon {
  margin-left: -6px;
}



.active-blue {
  /*background-color: rgb(24, 66, 144) !important; /* 蓝色 */
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
  border: none;
  color: white;
  font-size: 1.2rem;
  margin: 0;
  padding: 0.75rem 1.5rem;
  cursor: pointer;
  transition: background 0.3s;
  border-top-left-radius: 10px;
  border-top-right-radius: 10px;
  z-index: 1;
  position: relative;
  width: 100%; 
  background-color: transparent;
}

.tabs button.active {
  background-color: var(--button-color);
}

.tabs button:not(.active) {
  background: rgba(24, 66, 144, 0.1) !important;
  color: black !important;
}

.tabs button:hover {
  background-color: rgba(24, 66, 144, 0.7); 
  color: white; 
}

.page-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;  
  min-height: 100vh; 
}

.card-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
  min-height: 100vh; 
  /*background-image: url('/Images/bg.png');*/
  /*background-size: cover;  */
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
}

.about-section {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: flex-start;
  color: white;
  padding: 2rem;
  width: 100%; 
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

<template>
  <div class="page-container">
    <!-- 學院選單 -->
    <CollegeSelector v-if="!isCollegeSelected" :tabs="tabs" @select-college="selectCollege" />

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

          <!-- 各學院內容 -->
          <div v-if="currentTab === 'info'" class="about-section">
            <SectionContent
              :image-src="'/Images/im.png'"
              :description="`落實創新服務、培育立型人才。<br /> 培育不同層面之務實致用人才，滿足產業界的需求。`"
              :items="infoItems"
              :active-tab="currentDept"
              @change-tab="changeTab"
            />
          </div>

          <div v-else-if="currentTab === 'business'" class="about-section">
            <SectionContent
              :image-src="'/Images/business.png'"
              :description="`專業創新之商業跨域人才培育基地。<br /> 培育專業創新與敬業當責之商業管理人才。`"
              :items="businessItems"
              :active-tab="currentDept"
              @change-tab="changeTab"
            />
          </div>

          <div v-else-if="currentTab === 'design'" class="about-section">
            <SectionContent
              :image-src="'/Images/design.png'"
              :description="`培育跨領域整合設計人才，成為台灣中部文創與關懷設計中心。<br /> 主動積極與追求績效。`"
              :items="designItems"
              :active-tab="currentDept"
              @change-tab="changeTab"
            />
          </div>

          <div v-else-if="currentTab === 'smart'" class="about-section">
            <SectionContent
              :image-src="'/Images/intelligence.png'"
              :description="`朝向智慧產業技術跨領域永續發展。<br /> 發展智慧商業、智慧生產與智慧醫療三大領域。`"
              :items="smartItems"
              :active-tab="currentDept"
              @change-tab="changeTab"
            />
          </div>

          <div v-else-if="currentTab === 'language'" class="about-section">
            <SectionContent
              :image-src="'/Images/languages.png'"
              :description="`培養中、英、日語文應用及數位商務人才。<br /> 紮根本土並接軌國際，提升學生文字、數位、外語、商業應用等能力。`"
              :items="languageItems"
              :active-tab="currentDept"
              @change-tab="changeTab"
            />
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
import CollegeSelector from './CollegeSelector.vue';
import teeest from './teeest.vue';
import SectionContent from './SectionContent.vue'; // 将内容组件化

export default {
  name: 'Introduce',
  components: {
    CollegeSelector,
    teeest,
    SectionContent
  },
  data() {
    return {
      currentTab: '',
      currentDept: '',
      isCollegeSelected: false,
       backgroundColor: '',
      tabs: [
        { name: '資訊與流通學院', type: 'info', icon: 'fi fi-bs-computer', color: '#83C3DA' },
        { name: '商學院', type: 'business', icon: 'fi fi-bs-corporate', color: '#8A95CA' },
        { name: '設計學院', type: 'design', icon: 'fi fi-br-paintbrush-pencil', color: '#B69DD5' },
        { name: '智慧產業學院', type: 'smart', icon: 'fi fi-rs-microchip-ai', color: '#9FB6C6' },
        { name: '語文學院', type: 'language', icon: 'fi fi-rr-book-font', color: '#3497CC' }
      ],
      infoItems: [
        { text: '資訊管理系', link: 'teeest', color: '#5A7D8C' },
        { text: '資訊工程系', link: 'cs', color: '#5A7D8C' },
        { text: '流通管理系', link: 'logistics', color: '#5A7D8C' }
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
  selectCollege(tabType, dept) {
    this.currentTab = tabType;
    this.currentDept = dept || this.getDefaultDept(tabType);
    this.isCollegeSelected = true;

    // 设置背景颜色
    const selectedTab = this.tabs.find(tab => tab.type === tabType);
    if (selectedTab) {
      this.backgroundColor = selectedTab.color;
    }
  },
  changeTab(tabType) {
    this.currentTab = tabType;
    this.currentDept = this.getDefaultDept(tabType);

    // 设置背景颜色
    const selectedTab = this.tabs.find(tab => tab.type === tabType);
    if (selectedTab) {
      this.backgroundColor = selectedTab.color;
    }
  },
  getDefaultDept(college) {
    const defaultDepts = {
      info: 'teeest',
      business: 'international',
      design: 'business',
      smart: 'business-management',
      language: 'english'
    };
    return defaultDepts[college] || '';
  },
  goBack() {
    this.isCollegeSelected = false;
  }
},
  mounted() {
    const queryCollege = this.$route.query.college;
  const queryDept = this.$route.query.dept;

  if (queryCollege) {
    this.currentTab = queryCollege;
    this.currentDept = queryDept || this.getDefaultDept(queryCollege);

    // 设置背景颜色
    const selectedTab = this.tabs.find(tab => tab.type === queryCollege);
    if (selectedTab) {
      this.backgroundColor = selectedTab.color;
    }

    this.isCollegeSelected = true; // 显示学院内容
  }

  // 如果传递了dept参数，直接调用changeTab以显示特定科系
  if (queryDept) {
    this.changeTab(queryDept, queryCollege);
  }
  }
}
</script>

<style scoped>
@import url('https://cdn.jsdelivr.net/npm/@flaticon/flaticon-uicons/css/all/all.css');

.page-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;  
  min-height: 100vh; 
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

.card-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
  min-height: 100vh; 
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
</style>

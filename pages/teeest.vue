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
        <div v-if="currentDetail === '科系特色'">
          <h3>科系特色</h3>
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
        <div v-if="currentDetail === '師資'">
          <h3>資管師資</h3>
          <h3>系主任</h3>
          <p>姜琇森</p>
          <h3>教授</h3>
          <p>黃秀美	陳大仁	蕭國倫	柯志坤	黃天麒	連俊瑋	林真伊	王健亞</p>
          <h3>副教授</h3>
          <p>王淑玲	廖士寬	駱榮問	張玲娥	何瑞枝	林文彥	侯幸雨 	林冠妤 </p>
          <h3>助理教授</h3>
          <p>羅張瓊誼	邱淑芬	許雯絞	戴偉勝	周殷菀	曾建維 李銘峰 林佩璇 	陳美鐘 </p>
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
      items: ['科系特色', '課程', '專題成果', '師資'],
      currentDetail: ''
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
    }
  },
  mounted() {
    
    this.moveAndShowIntroduction(this.items[0]);
    this.isActive = true; 
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

.detail-container .detail {
  font-size: 16px;
  font-weight: bold;
  color: rgb(0, 0, 0);

  line-height: 1.6;
  word-break: break-word; 
  white-space: normal; 
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
  color: rgb(255, 255, 255);
}
</style>

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
        <div v-if="currentDetail === '大一'">
          <h3>大一在幹嘛?</h3>
          <p>剛踏入新的學校、周圍有新的同學老師，要重新規劃許多事情，例如時間表;時間表可以幫助自己有效的利用時間，包含課程安排、社團活動、社交時間等等。<br>同時也非常建議在剛入學時就多多的結交老師和朋友，保持良好的關係，對於之後分組和尋找學習資源是非常有幫助的!<br>至於要不要打工就是看自己的需求，但打工是會占用非常多時間的，可以不用打工，還是不要比較好。</p>
          <h3>課程內容：</h3>
          <p>微積分、計算機概論、會計學、實用日語會話(一)、溝通與簡報、資訊網路</p>
          <h3>活動：</h3>
          <p>新生入學典禮、迎新晚會</p>
        </div>
        <div v-if="currentDetail === '大二'">
          <h3>大二該努力什麼?</h3>
          <p>首先有被當掉的課程最好能補修完就早點補修，能修課也盡量修課，後面的年級也是非常忙碌，不能覺得後面課程少就想說之後在修課，而進入大二後，團隊朋友之類的合作夥伴都有基本的認識，分組活動基本上都會有固定的隊友，可以不必跟自己的朋友分組，大學的朋友基本上不會因為分組沒有一起就翻臉，只會在一組時因為沒有做好工作而翻臉，畢竟之後有個大魔王-專題，所以可以先淺淺的物色一下以後的夥伴，早一點的可能二下就可以開始找了，找到合適的老師也可以跟老師溝通哦!</p>
          <h3>課程內容：</h3>
          <p>統計學、行銷/生產管理、財務/人力資源管理、資料結構、JAVA程式設計、資料庫管理系統、資訊網路、線性代數、英語會話、實用英文、作業系統、演算法、.NET程式設計、電腦繪圖、Linux系統實務</p>
          <h3>活動：</h3>
          <p>學術講座、企業參訪</p>
        </div>
        <div v-if="currentDetail === '大三'">
          <h3>大三忙什麼?</h3>
          <p>大二大三是最容易鬆懈的時期，困擾許多人的多益畢業門檻也可以從上學期開始修特定的課程，修過了畢業門檻就過了哦!還有通識的部分，假如在大一大二已經通過畢業門檻，也可以再多修一堂通識，這樣可以比別人少修一學期，可以擁有更多空堂，專題的部分請在下學期前找好老師擊夥伴，快的人在大二暑假已經找玩，暑假在接受老師的訓練了!提早準備一定是非常好的，同時也要注意一下自己證照的部分，時間很快就過了，一直想說之後在考就來不及了，有些課程老師會幫同學報名證照，請把握機會!</p>
          <h3>課程內容：</h3>
          <p>科技英文、物聯網概論、物件導向系統分析、管理資訊系統、企業資源規劃、多媒體系統、人機介面設計、資訊倫理、軟體工程、資料庫專題、跨平台程式設計、巨量資料分析、知識管理、顧客關係管理、網路規劃與管理、伺服器建置與管理、行動程式設計、資訊安全、網際系統設計</p>
          <h3>活動：</h3>
          <p>實習機會、校外參訪</p>
        </div>
        <div v-if="currentDetail === '大四'">
          <h3>大四了???</h3>
          <p>時間很快就來到大四，大四需要做甚麼，相信各位都知道，補課、補證照、實習、專題等等，所以前面把課都修完、證照、畢業門檻等等瑣事全部處理完的人就只需要面對專題和實習，專題會帶給我們很大的壓力及挑戰，跟組員吵架是一定會有的，但請保持理智，這個痛苦的時期堅持到大四上就好了，大四下就是跟社會接軌或是繼續升學的時期，可以好好的跟大學說再見。</p>
          <h3>課程內容：</h3>
          <p>資訊管理實務專題、專業證照、校外實習、產業實習、企業實習、產業專業實習</p>
          <h3>活動：</h3>
          <p>畢業典礼、就业辅导</p>
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
      this.showDetail = true;
      this.isMoved = true;
      this.isActive = false;
    }
  },
  mounted() {
    
    this.moveAndShowIntroduction(this.items[0]);
    this.isActive = false; 
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

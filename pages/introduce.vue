<template>
  <div class="container">
    <header>
      <!-- 這裡可以插入標題或導航欄 -->
    </header>
    <div class="info">
      資訊管理系 學生數: 249, 教師數: 27
    </div>
    <main>
      <div id="tree-container" :class="{ show: treeContainerVisible }">
        <img
          id="tree-image"
          :src="currentStage.image"
          alt="大一 小苗"
          @click="nextStage"
          :class="{ show: treeImageVisible }"
        />
        
      </div>
      <div id="leaves-container" :class="{ show: leavesContainerVisible }">
          <div
            v-for="(leaf, index) in currentStage.leaves"
            :key="index"
            class="leaf"
            @click="showInfo(leaf.info)"
          >
            {{ leaf.text }}
          </div>
        </div>
    </main>
    <div id="info-modal" class="modal" :style="{ display: modalVisible ? 'block' : 'none' }">
      <div class="modal-content" :class="{ show: modalContentVisible }">
        <span class="close-button" @click="closeModal">&times;</span>
        <div id="info-text" v-html="infoText"></div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      stages: [
        {
          image: "/images/im1.png",
          leaves: [
            { text: "大一課程內容和基礎活動。", info: "微積分<br>計算機概論<br>會計學<br>實用日語會話(一)<br>溝通與簡報<br>資訊網路" },
            { text: "學生作品展示1。", info: "學生作品展示1。" },
          ],
        },
        {
          image: "/images/im2.png",
          leaves: [
            { text: "大二進階課程和活動。", info: "統計學<br>行銷/生產管理<br>財務/人力資源管理<br>資料結構<br>JAVA程式設計<br>資料庫管理系統<br>資訊網路<br>線性代數<br>英語會話<br>實用英文<br>作業系統<br>演算法<br>.NET程式設計<br>電腦繪圖<br>Linux系統實務" },
            { text: "學生作品展示2。", info: "學生作品展示2。" },
          ],
        },
        {
          image: "/images/im3.png",
          leaves: [
            { text: "大三專業課程和實習機會。", info: "科技英文<br>物聯網概論<br>物件導向系統分析<br>管理資訊系統<br>企業資源規劃<br>多媒體系統<br>人機介面設計<br>資訊倫理<br>軟體工程<br>資料庫專題<br>跨平台程式設計<br>巨量資料分析<br>知識管理<br>顧客關係管理<br>網路規劃與管理<br>伺服器建置與管理<br>行動程式設計<br>行動程式設計<br>資訊安全<br>網際系統設計" },
            { text: "專業項目展示。", info: "專業項目展示。" },
          ],
        },
        {
          image: "/images/year4.png",
          leaves: [
            { text: "大四畢業設計、研究項目和就業輔導。", info: "資訊管理實務專題<br>專業證照<br>校外實習<br>產業實習<br>企業實習<br>產業專業實習" },
            { text: "畢業作品和研究成果展示。", info: "畢業作品和研究成果展示。" },
          ],
        },
      ],
      currentStageIndex: 0,
      treeContainerVisible: false,
      treeImageVisible: false,
      leavesContainerVisible: false,
      modalVisible: false,
      modalContentVisible: false,
      infoText: "",
    };
  },
  computed: {
    currentStage() {
      return this.stages[this.currentStageIndex];
    },
  },
  methods: {
    nextStage() {
      this.treeImageVisible = false;
      this.leavesContainerVisible = false;
      setTimeout(() => {
        this.currentStageIndex = (this.currentStageIndex + 1) % this.stages.length;
        this.renderStage();
      }, 1000); // 等待1秒以確保淡出效果完成
    },
    renderStage() {
      setTimeout(() => {
        this.treeImageVisible = true;
        setTimeout(() => {
          this.leavesContainerVisible = true;
        }, 300);
      }, 300); // 延遲300毫秒後開始淡入效果
    },
    showInfo(info) {
      this.infoText = info;
      this.modalVisible = true;
      setTimeout(() => {
        this.modalContentVisible = true;
      }, 10);
    },
    closeModal() {
      this.modalContentVisible = false;
      setTimeout(() => {
        this.modalVisible = false;
      }, 500);
    },
  },
  mounted() {
    this.renderStage();
    setTimeout(() => {
      this.treeContainerVisible = true;
    }, 10);
  },
};
</script>

<style scoped>
body {
  font-family: Arial, sans-serif;
  background-color: rgb(24, 66, 144);
  margin: 0;
  padding: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  width: 100%;
  box-sizing: border-box; /* 確保padding和border不會增加元素的寬度 */
}

.container {
  width: 100%;
  height: 100vh;
  max-width: 1200px;
  margin: 0 auto;
  padding: 10px;
}

header {
  width: 100%;
}

.info {
  text-align: center;
  color: white;
  font-size: 20px;
  margin: 20px 0;
}

#tree-container {
  height: 100%;
  position: relative;
  text-align: center;
  margin-top: 20px;
  opacity: 0; /* 初始時設置為透明 */
  transition: opacity 1s ease; /* 使用透明度過渡 */
  width: 100%; /* 確保容器寬度 */
  max-width: 400px; /* 確保容器最大寬度 */
}

#tree-container.show {
  opacity: 1; /* 顯示時恢復透明度 */
}

#tree-image {
  width: 100%;
  height: 100%;
  cursor: pointer; /* 確保圖片顯示為可點擊 */
  opacity: 0; /* 初始時設置為透明 */
  transition: opacity 1s ease; /* 使用透明度過渡 */
}

#tree-image.show {
  opacity: 1; /* 顯示時恢復透明度 */
}

#leaves-container {
  display: flex;
  justify-content: space-between;
  align-items: center;
  
  width: 100%; /* 調整文字框的寬度 */
  bottom: 100px;
  margin: auto; /* 讓文字框置中 */
  opacity: 0; /* 初始時設置為透明 */
  transition: opacity 1s ease; /* 使用透明度過渡 */
}

#leaves-container.show {
  opacity: 1; /* 顯示時恢復透明度 */
}

.leaf {
  background-color: #98fb98;
  padding: 10px 10px;
  margin: 10px;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s, transform 0.3s;
  flex: 1; /* 讓每個葉子按鈕平分寬度 */
  text-align: center;
}

.leaf:hover {
  background-color: #32cd32;
  transform: scale(1.1);
}

.modal {
  display: none;
  position: fixed;
  z-index: 1;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  overflow: auto;
  background-color: rgba(0, 0, 0, 0.4);
}

.modal-content {
  background-color: white;
  margin: 15% auto;
  padding: 20px;
  border: 1px solid #888;
  width: 80%;
  max-width: 500px;
  text-align: center;
  opacity: 0; /* 初始時設置為透明 */
  transition: opacity 0.5s ease; /* 使用透明度過渡 */
}

.modal-content.show {
  opacity: 1; /* 顯示時恢復透明度 */
}

.close-button {
  color: #aaa;
  float: right;
  font-size: 28px;
  font-weight: bold;
}

.close-button:hover,
.close-button:focus {
  color: black;
  text-decoration: none;
  cursor: pointer;
}

#info-text {
  font-size: 18px;
  line-height: 1.6;
  text-align: left;
  white-space: pre-wrap; /* 保持文本中的換行 */
}
</style>

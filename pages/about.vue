<template>
  <div>
    <div v-if="!isComplete">
      <div id="board">
        <img
          v-for="tile in boardTiles"
          :src="tile.src"
          :key="tile.key"
          :alt="tile.alt"
          @touchstart="onTouchStart"
        />
      </div>
      <h2 style="text-align: center;">Turns: <span>{{ turns }}</span></h2>
      <div id="pieces">
        <img
          v-for="piece in pieces"
          :src="piece.src"
          :key="piece.key"
          :alt="piece.alt"
          @touchstart="onTouchStart"
        />
      </div>
      <div class="center-content">
        <UButton color="white" variant="solid" @click="istoggleon = !istoggleon">Tips</UButton>
        <p v-show="istoggleon">點擊下方的照片與上方的照片即可交換照片，完成拼圖後會有神秘力量</p>
      </div>
    </div>
    <div v-if="isComplete" class="container">
      <img src="/images/puzzle.png" class="card-img" >
      <div>
        <h1>全校各棟大樓編號表</h1>
        <p>
舉例:2401--2-資訊館，4樓，01號教室<br><br>
1開頭-行政大樓：位於三民路校門口進來的左側第一棟，負責處理學校的行政事務，包括註冊、課程安排和學生服務等。<br><br>

2開頭-資訊館：位於行政大樓的左側，走到走廊的盡頭，左轉即可看到，整棟大樓有電梯。提供教室和辦公室，供教職員和學生使用。<br><br>

3開頭-中正大樓：從三民路校門口左側第二棟，新生體檢是在這棟進行。主要教學大樓，內有教室和實驗室。

活動中心：從中正大樓過去，靠近昌明樓的左側，主要用於學生社團活動，包括課程和輔導組。<br><br>

4開頭-昌明樓：位於校園中心，從三民路校門口直走即到，主要設有設計群的教室和辦公室。核心教學區之一，支援設計相關課程。<br><br>

5開頭-翰英樓：靠近操場和司令台的一整排，提供相關設施和服務。<br><br>

6開頭-弘業樓：從三民路校門口進來的右側，位於體育館樓上那一整棟。主要的教學和研究設施。<br><br>

7開頭-中商大樓：位於中華路，穿過操場，經過翰英樓，圖書館就在附近。用於商業相關課程的教學設施。<br><br>

8開頭-奇秀樓：體育館右側轉直走，靠近男宿，位於操場旁邊，1樓可借用體育器材。提供體育器材和相關設施。
</p>
      <div class="center-content">
      <a href="/qna" class="qna-link">想了解更多點擊這裡到 QnA</a>
    </div>
    </div>
    </div>
    
    
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';

const rows = 3;
const columns = 4;

const istoggleon = ref(false);
const currTile = ref(null);
const otherTile = ref(null);
const boardTiles = ref([]);
const pieces = ref([]);
const turns = ref(0);
const isComplete = ref(false);

const initializeBoard = () => {
  for (let r = 0; r < rows; r++) {
    for (let c = 0; c < columns; c++) {
      const tile = { src: "/images/blank.jpg", key: `tile-${r}-${c}`, alt: `Tile ${r * columns + c + 1}` };
      if ((r === 0 && c === 0) || (r === 2 && c === 2)) {
        tile.src = `/images/${1 + r * columns + c}.png`;
      }
      boardTiles.value.push(tile);
    }
  }
};

const initializePieces = () => {
  const piecesArray = [];
  for (let i = 2; i <= rows * columns; i++) {
    if (i === 1 || i === 11) {
      continue;
    }
    const piece = { src: `/images/${i}.png`, key: `piece-${i}`, alt: `Piece ${i}` };
    piecesArray.push(piece);
  }

  piecesArray.reverse();

  for (let i = 0; i < piecesArray.length; i++) {
    const j = Math.floor(Math.random() * piecesArray.length);
    [piecesArray[i], piecesArray[j]] = [piecesArray[j], piecesArray[i]];
  }

  pieces.value = piecesArray;
};

const onTouchStart = (event) => {
  if (!currTile.value) {
    currTile.value = event.target;
  } else {
    otherTile.value = event.target;
    if (currTile.value !== otherTile.value) {
      swapTiles();
    }
  }
};


const swapTiles = () => {
  const tempSrc = currTile.value.src;
  currTile.value.src = otherTile.value.src;
  otherTile.value.src = tempSrc;

  currTile.value = null;
  otherTile.value = null;

  turns.value += 1;
  checkPuzzleCompletion();
};

const checkPuzzleCompletion = () => {
  const expectedOrder = [];
  for (let i = 1; i <= rows * columns; i++) {
    expectedOrder.push(`${i}.png`);
  }

  console.log('Expected Order:', expectedOrder);
  console.log('Board:' ,board);
  isComplete.value = expectedOrder.every((src, index) => {
    const imgElement = document.querySelector(`#board img:nth-child(${index + 1})`);
    if (imgElement) {
      
      const boardFileName = imgElement.src.split('/').pop();
      return src === boardFileName;
    } else {
      return false; 
    }
  });
  
};


  if (isComplete.value) {
    console.log('Puzzle completed!');
  }





onMounted(() => {
  initializeBoard();
  initializePieces();
});
</script>

<style scoped>

#board {
  width: 350px;
  height: 260px;
  border: 2px solid lightskyblue;
  margin: 0 auto;
  display: flex;
  flex-wrap: wrap;
}

#board img {
  width: 85px;
  height: 85px;
  border: 0.5px solid lightblue;
}

#pieces {
  width: auto;
  height: auto;
  border: 2px solid lightskyblue;
  margin: 0 auto;
  display: flex;
  flex-wrap: wrap;
}

#pieces img {
  width: 80px;
  height: 80px;
  border: 0.5px solid lightblue;
}

.container {
  background-color: white;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
  text-align: center;
  opacity: 0;
  animation: fadeIn 1s forwards;
  perspective: 100%;
  height: auto;
  max-width: 100%;
  box-sizing: border-box;
  overflow: auto; 
  padding-bottom: 80px;
  position: relative;
  z-index: 999;
  overflow: visible;
}

.center-content {
  text-align: center;
  margin-top: 20px;
  padding: 10px; 
}
.card-img{
  border-radius: 10px;
  width: 100%;
  height: auto;
  animation: fadeIn 1s forwards;
  transition: all .15s ease;
  transform: translateY(20px) scale(1);
  box-shadow: 0 0px 5px rgba(0, 0, 0, 0.3);
}
.card-img:hover{
  transform: translateY(40px) scale(1.1);
  box-shadow: 0 20px 40px rgba(0, 0, 0, 0.3);
}
p {
    font-size: 0.9em;
    line-height: 1.8;
    color: #444;
    background-color: #fff;
    border: 1px solid #ddd;
    padding: 15px;
    border-radius: 10px;
    margin: 20px auto;
    max-width: 600px;
    box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
    transition: all 0.3s ease;
    opacity: 0;
    animation: fadeIn 3s forwards;
}


p:hover {
    box-shadow: 0 10px 20px rgba(0, 0, 0, 0.2);
    transform: translateY(-5px);
}
.qna-link {
  color: red; 
  text-decoration: underline;
  cursor: pointer;
  display: inline-block; /* 讓連結更容易被點擊 */
  margin-top: 20px; /* 上邊距確保連結不會緊貼其他內容 */
}

@keyframes fadeIn {
    from {
        opacity: 0;
        transform: translateY(20px);
    }
    to {
        opacity: 1;
        transform: translateY(0);
    }
}


</style>

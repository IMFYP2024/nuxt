<template>
  <div>
    <Modal
      :title="modalTitle"
      :showModal="showModal"
      @closing="closeModal"
    >
      <template #itemText>
        <div>
          <img src="public/Images/puzzle.png" alt="Sample Photo" class="modal-image"/>
          <p>這個是拼圖完成後的模樣，在關閉視窗後，請點擊下方的照片與上方的照片即可交換照片，可隨時點擊圖標來反復觀看範例圖</p>
        </div>
      </template>
    </Modal>
    <div v-if="!isComplete" class="container">
      <h1>關於學校</h1>
      <div id="board">
        <img
          v-for="tile in boardTiles"
          :src="tile.src"
          :key="tile.key"
          :alt="tile.alt"
          @touchstart="onTouchStart"
        />
      </div>
      <div class="turns-container">
        <h2>Turns: <span>{{ turns }}</span></h2>
        <div class="icon-trigger" @click="showModal = true">
          <Icon name="icon-park-solid:tips-one" class="large-icon" />
        </div>
      </div>
      
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
        <p v-show="istoggleon"></p>
      </div>
    </div>
    <div v-if="isComplete" class="container">
      <img src="public/Images/puzzle.png" class="card-img">
      <div>
        <h1>學校故事</h1>
        <p>陽光大學在一個寧靜的下午所創立的，當時候陽光透過樹葉灑下斑駁的光影，微風輕輕拂過，帶來了花香的氣息。這樣的時刻，總是讓人心情愉悅，
          彷彿所有的煩惱都隨風而去。生活中的美好，往往藏在這些簡單而平凡的瞬間裡，只要我們用心去感受，就能找到無限的快樂。</p>
        <!-- <p>我們學校是1919年6月所創立的至今已經有一百零五年的歷史了，我們學校的全校學生總數: {{ totalStudentCount }} 人，教師總數: {{ totalTeacherCount }} 人，
          由此可見我們學校的師資以及學生是很龐大的。學校有幾個校區，其中三民校區是最為人所熟知的校區之一，該校區有多棟建築物，如行政大樓、資訊館、中正大樓、活動中心、昌明樓等，為學生提供優質的學習環境。</p> -->
      </div>
      <router-link to="/" class="nav-link">
          <Icon name="streamline:return-2-solid" class="large-icon" />
        </router-link>
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
const modalTitle = ref('範例圖');
const showModal = ref(true);

const totalStudentCount = ref(0);
const totalTeacherCount = ref(0);

const closeModal = () => {
  showModal.value = false;
};

const initializeBoard = () => {
  for (let r = 0; r < rows; r++) {
    for (let c = 0; c < columns; c++) {
      const tile = { src: "/Images/blank.jpg", key: `tile-${r}-${c}`, alt: `Tile ${r * columns + c + 1}` };
      if ((r === 0 && c === 0) || (r === 2 && c === 2)) {
        tile.src = `/Images/${1 + r * columns + c}.png`;
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
    const piece = { src: `/Images/${i}.png`, key: `piece-${i}`, alt: `Piece ${i}` };
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

  isComplete.value = expectedOrder.every((src, index) => {
    const imgElement = document.querySelector(`#board img:nth-child(${index + 1})`);
    if (imgElement) {
      const boardFileName = imgElement.src.split('/').pop();
      return src === boardFileName;
    } else {
      return false; 
    }
  });

  if (isComplete.value) {
    console.log('Puzzle completed!');
  }
};

const fetchData = async () => {
  try {
    const response = await fetch('http://163.17.135.197/nuxt_api.php');
    if (!response.ok) {
      throw new Error('Network response was not ok');
    }
    const result = await response.json();

    totalStudentCount.value = result.reduce((sum, item) => sum + Number(item.student_count), 0);
    totalTeacherCount.value = result.reduce((sum, item) => sum + Number(item.teacher_count), 0);
    console.log('Total Student Count:', totalStudentCount.value);
    console.log('Total Teacher Count:', totalTeacherCount.value);
  } catch (error) {
    console.error('There was a problem with the fetch operation:', error);
  }
};


onMounted(() => {
  initializeBoard();
  initializePieces();
  fetchData();
});
</script>

<style scoped>
/* Your existing styles */
.large-icon {
  width: 36px;
  height: 36px;
  color: black;
}
.turns-container{
    display: flex;
    height: 15vh;
    flex-direction: row;
    align-items: center;
    align-content: center;
    justify-content: space-evenly;
}
.turns-container h2{
  font-size: 30px;
}
.modal-image {
  max-width: 100%; 
  height: auto; 
  display: block;
  margin: 0 auto; 
}

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
    position: relative;
}

@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(-30px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}
</style>
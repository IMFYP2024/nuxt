<template>
    <div>
      <div id="board">
        <img
          v-for="tile in boardTiles"
          :src="tile.src"
          :key="tile.key"
          :alt="tile.alt"
          draggable="true"
          @dragstart="dragStart"
          @dragover="dragOver"
          @dragenter="dragEnter"
          @dragleave="dragLeave"
          @drop="dragDrop"
          @dragend="dragEnd"
        />
      </div>
      <h2>Turns: <span>{{ turns }}</span></h2>
      <div id="pieces">
        <img
          v-for="piece in pieces"
          :src="piece.src"
          :key="piece.key"
          :alt="piece.alt"
          draggable="true"
          @dragstart="dragStart"
          @dragover="dragOver"
          @dragenter="dragEnter"
          @dragleave="dragLeave"
          @drop="dragDrop"
          @dragend="dragEnd"
        />
      </div>
      <div id="text" v-show="isComplete">恭喜！你完成了拼图！</div>
    </div>
  </template>
  
  <script setup>
  import { ref } from 'vue';
  
  const rows = 3;
  const columns = 4;
  
  const boardTiles = ref([]);
  const pieces = ref([]);
  const currTile = ref(null);
  const otherTile = ref(null);
  const turns = ref(0);
  const isComplete = ref(false);
  
  const initializeBoard = () => {
    for (let r = 0; r < rows; r++) {
      for (let c = 0; c++) {
        boardTiles.value.push({
          src: "/images/blank.jpg",
          key: `tile-${r}-${c}`,
          alt: `Tile ${r * columns + c + 1}`,
        });
      }
    }
  };
  
  const initializePieces = () => {
    const piecesArray = [];
    for (let i = 1; i <= rows * columns; i++) {
      piecesArray.push({ src: `/images/${i}.png`, key: `piece-${i}`, alt: `Piece ${i}` });
    }
  
    piecesArray.reverse();
  
    for (let i = 0; i < piecesArray.length; i++) {
      const j = Math.floor(Math.random() * piecesArray.length);
      [piecesArray[i], piecesArray[j]] = [piecesArray[j], piecesArray[i]];
    }
  
    pieces.value = piecesArray;
  };
  
  const dragStart = (event) => {
    currTile.value = event.target;
  };
  
  const dragOver = (event) => {
    event.preventDefault();
  };
  
  const dragEnter = (event) => {
    event.preventDefault();
  };
  
  const dragLeave = () => {};
  
  const dragDrop = (event) => {
    otherTile.value = event.target;
  };
  
  const dragEnd = () => {
    if (currTile.value.src.includes("blank")) {
      return;
    }
  
    // 交换图片
    const tempSrc = currTile.value.src;
    currTile.value.src = otherTile.value.src;
    otherTile.value.src = tempSrc;
  
    // 增加次数
    turns.value++;
  
    checkPuzzleCompletion();
  };
  
  const checkPuzzleCompletion = () => {
    const expectedOrder = [];
    for (let i = 1; i <= rows * columns; i++) {
      expectedOrder.push(`/images/${i}.png`);
    }
  
    const board = boardTiles.value.map((tile) => tile.src);
  
    isComplete.value = expectedOrder.every((src, index) => src === board[index]);
  
    if (isComplete.value) {
      document.getElementById("text").style.display = "block";
    }
  };
  
  initializeBoard();
  initializePieces();
  </script>
  
  <style scoped>
  #board {
    width: 400px;
    height: 300px;
    border: 2px solid purple;
    margin: 0 auto;
    display: flex;
    flex-wrap: wrap;
  }
  
  #board img {
    width: 98px;
    height: 98px;
    border: 0.5px solid lightblue;
  }
  
  #pieces {
    width: 1200px;
    height: 100px;
    border: 2px solid purple;
    margin: 0 auto;
    display: flex;
    flex-wrap: wrap;
  }
  
  #pieces img {
    width: 98px;
    height: 98px;
    border: 0.5px solid lightblue;
  }
  
  #text {
    display: none;
  }
  </style>
  
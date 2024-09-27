<template>
  <div class="college-selector-page" >
    <h2>學院介紹</h2>
    <div class="college-grid">
      <div 
        v-for="(tab, index) in tabs" 
        :key="index" 
        :class="'college-item item-' + (index + 1)"
        @click="navigateToCollege(tab.type)"
      >
        <i :class="tab.icon" class="college-icon"></i>
        <p>{{ tab.name }}</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'CollegeSelectorPage',
  data() {
    return {
      tabs: [
        { name: '資訊學院', type: 'info', icon: 'fi fi-bs-computer', color: '#83C3DA' },
        { name: '商學院', type: 'business', icon: 'fi fi-bs-corporate', color: '#8A95CA' },
        { name: '設計學院', type: 'design', icon: 'fi fi-br-paintbrush-pencil', color: '#B69DD5' },
        { name: '智慧產業學院', type: 'smart', icon: 'fi fi-rs-microchip-ai', color: '#9FB6C6' },
        { name: '語文學院', type: 'language', icon: 'fi fi-rr-book-font', color: '#3497CC' }
      ]
    };
  },
  methods: {
    navigateToCollege(tabType) {
   
      const url = `/deintroduce?college=${tabType}`;
      window.location.href = url;
    }
  }
}
</script>

<style>


@media (max-height: 670px) {
  .college-selector-page h2 {
    margin-top: 15%; /* 調整標題位置 */
    margin-bottom: 100px;
    z-index: 10;
  }
}

@media (min-height: 671px) and (max-height: 883px) {
  .college-selector-page h2 {
    margin-top: 35%; /* 調整標題位置 */
    margin-bottom: 100px;
    z-index: 10;
  }
}

@media (min-height: 884px) {
  .college-selector-page h2 {
    margin-top: 55%; /* 調整標題位置 */
    margin-bottom: 100px;
    z-index: 10;
  }
}



.college-selector-page {
  display: flex;
  flex-direction: column;
  align-items: center;
  background-image: url('/Images/bg.jpg');
  height: 100vh;
    width: 100vw;
}

.college-grid {
  position: relative;
  width: 200px;
  height: 250px;
}

.college-item {
  position: absolute;
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

.item-1 {
  background-color: #83C3DA;
  top: 15%;
  left: 25%;
  transform: translate(-50%, -50%) rotate(45deg);
}
.item-1 .college-icon {
  margin-left: -28%; 
  margin-TOP: 13%; 
  margin-bottom: 35%; 
}

.item-2 {
  background-color: #8A95CA;
  top: 116%;
  left: -2%;
  transform: translate(-50%, -50%) rotate(45deg);
}
.item-2 .college-icon {
  margin-left: -20%; 
  margin-TOP: 10%; 
  margin-bottom: 35%; 
}
.item-3 {
  background-color: #B69DD5;
  top: 75%;
  left: 50%;
  transform: translate(-50%, -50%) rotate(45deg);
}
.item-3 .college-icon {
  margin-left: -20%; 
  margin-TOP: 10%; 
  margin-bottom: 35%; 
}
.item-4 {
  background-color: #9FB6C6;
  top: 37%;
  left: 100%;
  transform: translate(-50%, -50%) rotate(45deg);
}
.item-4 .college-icon {
  margin-left: -20%; 
  margin-TOP: 10%; 
  margin-bottom: 35%; 
}

.item-5 {
  background-color: #3497CC;
  top: 133%;
  left: 75%;
  transform: translate(-50%, -50%) rotate(45deg);
}
.item-5 .college-icon {
  margin-left: -20%; 
  margin-TOP: 10%; 
  margin-bottom: 35%; 
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
  margin-bottom: 50PX;
}

.college-item p {
  margin: 10%;
  font-size: 1rem;
  line-height: 1.2;
  margin-top: -40%;
  margin-right: -5%;
  margin-left: 15%;
  margin-bottom: 20%;
}
</style>

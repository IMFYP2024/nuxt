<template>
    <div>
      <div id="ar-container"></div>
      <div id="distance-display">距離： {{ distance.toFixed(2) }} 公尺</div>
      <div id="side-menu" :class="{ open: isMenuOpen }">
        <div class="search-container">
          <input type="text" v-model="searchQuery" placeholder="教室編號" @keyup.enter="handleSearch"/>
          <button @click="handleSearch" class="search-icon-button">
            <i class="fas fa-search"></i>
          </button>
        </div>
        <div v-if="showResults" class="serschtext">
          <div>大樓：{{ building }}</div>
          <div>樓層：{{ floor }}</div>
          <div>編號：{{ roomNumber }}</div>
        </div>
        <ul class="toggle-container">
          <li class="toggle-dropdown">
            <button @click="toggleDropdown('target1')" :class="{ open: dropdown.target1 }">
              行政大樓<i class="fas fa-chevron-down arrow"></i>
            </button>
            <transition name="slide-fade">
              <ul v-show="dropdown.target1">
                <li class="s-dropdown">1101文書組</li>
                <li class="s-dropdown">1104保管組</li>
                <li class="s-dropdown">1105出納組</li>
                <li class="s-dropdown">1106營繕組</li>
                <li class="s-dropdown">1107環境與安全衛生組</li>
              </ul>
            </transition>
          </li>
          <li class="toggle-dropdown">
            <button @click="toggleDropdown('target2')" :class="{ open: dropdown.target2 }">
              資訊館<i class="fas fa-chevron-down arrow"></i>
            </button>
            <transition name="slide-fade">
              <ul v-show="dropdown.target2">
                <li class="s-dropdown">2101事務組</li>
                <li class="s-dropdown">2203網路工程組</li>
                <li class="s-dropdown">2204校務資訊組</li>
                <li class="s-dropdown">2305教學資訊組</li>
                <li class="s-dropdown">2406資訊管理系辦公室</li>
                <li class="s-dropdown">2505資訊工程系辦公室</li>
                <li class="s-dropdown">2805資訊與流通學院數位內容與教學組</li>
              </ul>
            </transition>
          </li>
          <li class="toggle-dropdown">
            <button @click="toggleDropdown('target3')" :class="{ open: dropdown.target3 }">
              中商大樓<i class="fas fa-chevron-down arrow"></i>
            </button>
            <transition name="slide-fade">
              <ul v-show="dropdown.target3">
                <li class="s-dropdown">圖書館</li>
                <li class="s-dropdown">7413會計資訊系辦公室</li>
                <li class="s-dropdown">7510應用統計系辦公室</li>
                <li class="s-dropdown">7711保險金融管理系辦公室</li>
                <li class="s-dropdown">7810企業管理系辦公室</li>
                <li class="s-dropdown">7923財政稅務系辦公室</li>
              </ul>
            </transition>
          </li>
          <li class="toggle-dropdown">
            <button @click="toggleDropdown('target4')" :class="{ open: dropdown.target4 }">
              弘業樓<i class="fas fa-chevron-down arrow"></i>
            </button>
            <transition name="slide-fade">
              <ul v-show="dropdown.target4">
                <li class="s-dropdown">6201流通管理系辦公室</li>
              </ul>
            </transition>
          </li>
          <li class="toggle-dropdown">
            <button @click="toggleDropdown('target5')" :class="{ open: dropdown.target5 }">
              中正大樓<i class="fas fa-chevron-down arrow"></i>
            </button>
            <transition name="slide-fade">
              <ul v-show="dropdown.target5">
                <li class="s-dropdown">3104B商業經營系辦公室</li>
                <li class="s-dropdown">3105國際事務處</li>
                <li class="s-dropdown">3215註冊組課務組</li>
                <li class="s-dropdown">3314綜合業務組</li>
                <li class="s-dropdown">3410語言中心</li>
                <li class="s-dropdown">3515應用英語系辦公室</li>
                <li class="s-dropdown">3615應用日語系辦公室</li>
                <li class="s-dropdown">3812國際會議廳</li>
                <li class="s-dropdown">3910應用中文系辦公室</li>
              </ul>
            </transition>
          </li>
          <li class="toggle-dropdown">
            <button @click="toggleDropdown('target6')" :class="{ open: dropdown.target6 }">
              昌明樓<i class="fas fa-chevron-down arrow"></i>
            </button>
            <transition name="slide-fade">
              <ul v-show="dropdown.target6">
                <li class="s-dropdown">4110健康中心</li>
                <li class="s-dropdown">4112衛保組</li>
                <li class="s-dropdown">4113A學生宿舍組</li>
              </ul>
            </transition>
          </li>
          <li class="toggle-dropdown">
            <button @click="toggleDropdown('target7')" :class="{ open: dropdown.target7 }">
              奇秀樓<i class="fas fa-chevron-down arrow"></i>
            </button>
            <transition name="slide-fade">
              <ul v-show="dropdown.target7">
                <li class="s-dropdown">8101體育室</li>
                <li class="s-dropdown">8102器材室</li>
              </ul>
            </transition>
          </li>
          <li class="toggle-dropdown">
            <button @click="toggleDropdown('target8')" :class="{ open: dropdown.target8 }">
              中技大樓<i class="fas fa-chevron-down arrow"></i>
            </button>
            <transition name="slide-fade">
              <ul v-show="dropdown.target8">
                <li class="s-dropdown">H414休閒系辦公室</li>
                <li class="s-dropdown">H602財金系辦公室</li>
              </ul>
            </transition>
          </li>
          <li><button @click="addCoordinate('target1', [24.149687287189632, 120.68297905956476])">中正樓</button></li>
          <li><button @click="addCoordinate('target2', [24.1514351979012, 120.68242524993458])">中商大樓</button></li>
          <li><button @click="addCoordinate('target3', [24.149761545708383, 120.68367990956797])">資訊樓</button></li>
          <li><button @click="addCoordinate('target4', [24.15046289505919, 120.68410126817396])">弘業樓</button></li>
          <li><button @click="addCoordinate('target5', [24.150411337619133, 120.68297407245603])">昌明樓</button></li>
          <li><button @click="addCoordinate('target6', [24.15120942332591, 120.68399093382135])">奇秀樓</button></li>
          <li><button @click="addCoordinate('target7', [24.151279396565393, 120.6829962759951])">翰英樓</button></li>
          <li><button @click="addCoordinate('target8', [24.15229293057009, 120.68384512885488])">中技大樓</button></li>


          <li><button @click="clearCoordinates">取消</button></li>
          
        </ul>
      </div>
      <button id="menu-toggle" :class="{ 'menu-open': isMenuOpen }" @click="toggleMenu"><i :class="toggleIcon"></i></button>
      <div id="map"></div>
    </div>
  </template>
  
  <script setup>
  import { ref, onMounted, onBeforeUnmount } from 'vue';
  
  useHead({
    script: [
      { src: 'https://aframe.io/releases/1.3.0/aframe.min.js' },
      { src: 'https://raw.githack.com/AR-js-org/AR.js/master/three.js/build/ar-threex-location-only.js' },
      { src: 'https://raw.githack.com/AR-js-org/AR.js/master/aframe/build/aframe-ar-nft.js' },
      { src: 'https://unpkg.com/aframe-look-at-component@0.8.0/dist/aframe-look-at-component.min.js' },
      { src: 'https://unpkg.com/leaflet/dist/leaflet.js' },
      { src: 'https://cdn.rawgit.com/donmccurdy/aframe-extras/v6.1.0/dist/aframe-extras.min.js' },
    ],
    link: [
      { rel: 'stylesheet', href: 'https://unpkg.com/leaflet/dist/leaflet.css' }
    ],
    meta: [
      {
        name: 'viewport',
        content: 'width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no'
      }
    ]
  });
  
  const distance = ref(0);
  const azimuth = ref(0);
  const create = ref(null);
  const isMenuOpen = ref(false);
  const searchQuery = ref('');
  let map = null;
  const coordinates = ref([]);
  let markers = [];
  let targetCoordinates = null;
  let userPosition = null;
  let watchId = null;
  let userMarker = null;
  let arEntity = null;
  const building = ref('');
  const floor = ref('');
  const roomNumber = ref('');
  const showResults = ref(false); 
  
  const toggleMenu = () => {
    isMenuOpen.value = !isMenuOpen.value;
    setTimeout(() => {
      toggleIcon.value = isMenuOpen.value ? 'fas fa-times' : 'fas fa-bars'; 
    }, 300);
  };
  const toggleIcon = ref('fas fa-bars'); 

  const dropdown = ref({
    target1: false,
    target2: false,
    target3: false,
    target4: false,
    target5: false,
    target6: false,
    target7: false,
    target8: false
  });

  
  const toggleDropdown = (target) => {
    dropdown.value[target] = !dropdown.value[target];
  };


  function haversineDistance(lat1, lon1, lat2, lon2) {
    const R = 6371e3;
    const φ1 = toRad(lat1);
    const φ2 = toRad(lat2);
    const Δφ = toRad(lat2 - lat1);
    const Δλ = toRad(lon2 - lon1);
  
    const a = Math.sin(Δφ / 2) * Math.sin(Δφ / 2) +
              Math.cos(φ1) * Math.cos(φ2) *
              Math.sin(Δλ / 2) * Math.sin(Δλ / 2);
    const c = 2 * Math.atan2(Math.sqrt(a), Math.sqrt(1 - a));
  
    const distance = R * c;
    return distance;
  }
  
  function toRad(degrees) {
    return degrees * Math.PI / 180;
  }
  
  
  
  const calculateDistance = () => {
    if (userPosition && targetCoordinates) {
      distance.value = haversineDistance(userPosition.lat, userPosition.lng, targetCoordinates[0], targetCoordinates[1]);
      console.log('距離為', distance.value);
    }
  };
  
  
  
  const addCoordinate = (targetId, coord) => {
    clearCoordinates();
    targetCoordinates = coord;
    if (userPosition) {
      calculateDistance();
      create.value(userPosition, targetCoordinates);
      
    } else {
      console.error('獲取不到使用者位置');
    }
  
    coordinates.value = [];
    markers.forEach(marker => {
      map.removeLayer(marker);
    });
    markers = [];
  
    coordinates.value.push(coord);
  
    const redIcon = new L.Icon({
      iconUrl: 'https://raw.githubusercontent.com/pointhi/leaflet-color-markers/master/img/marker-icon-2x-red.png',
      iconSize: [25, 41],
      iconAnchor: [12, 41],
      popupAnchor: [1, -34],
      shadowSize: [41, 41]
    });
    const newMarker = L.marker(coord, { icon: redIcon }).addTo(map);
  
    markers.push(newMarker);
  
    const target = document.getElementById(targetId);
    if (target) {
      target.setAttribute('visible', 'true');
    }
  };
  
  const clearCoordinates = () => {
    coordinates.value = [];
    markers.forEach(marker => {
      map.removeLayer(marker);
    });
    markers = [];
  
    const targets = document.querySelectorAll('a-entity[gps-new-entity-place]');
    targets.forEach(target => {
      target.setAttribute('visible', 'false');
    });
  
    const directionEntity = document.getElementById('direction');
    if (directionEntity) {
      directionEntity.parentNode.removeChild(directionEntity);
    }
    
    distance.value = 0;
    azimuth.value = 0;
    targetCoordinates = null;
  };
  
  const handleSearch = () => {
    const searchValue = searchQuery.value;

    if (searchValue.length >= 3) {
      const firstChar = searchValue.charAt(0);
      const secondChar = searchValue.charAt(1);
      const remainingChars = searchValue.slice(2);

      if (firstChar === '2') {
        building.value = '資訊樓';
      } else if (firstChar === '3') {
        building.value = '中正樓';
      }

      floor.value = secondChar;
      roomNumber.value = remainingChars;

      if (firstChar === '3') {
        addCoordinate('target1', [24.149687287189632, 120.68297905956476]);
      } else if (firstChar === '2') {
        addCoordinate('target3', [24.149761545708383, 120.68367990956797]);
      }
      showResults.value = true;
    }
  };
  
  onMounted(() => {
    map = L.map('map', {
        zoomControl: false, // 禁用放大縮小
        attributionControl: false // 禁用浮水印
    }).setView([24.1514351979012, 120.68242524993458], 15);
    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
      attribution: '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors'
    }).addTo(map);
  
    if (navigator.geolocation) {
      navigator.geolocation.getCurrentPosition((position) => {
        userPosition = {
          lat: position.coords.latitude,
          lng: position.coords.longitude
        };
        console.log('使用者經緯度', userPosition);
  
        userMarker = L.marker([userPosition.lat, userPosition.lng]).addTo(map)
          .bindPopup('我在這')
          .openPopup();
  
        watchId = navigator.geolocation.watchPosition((position) => {
          userPosition = {
            lat: position.coords.latitude,
            lng: position.coords.longitude
          };
          console.log('更新使用者經緯度', userPosition);
          userMarker.setLatLng([userPosition.lat, userPosition.lng]);
          calculateDistance();
          // if (targetCoordinates) {
          //       create.value(userPosition, targetCoordinates);
          //       console.log('目標',userPosition,targetCoordinates)
          //   }
        }, (error) => {
          console.error('更新使用者位置失敗', error);
        });
      }, (error) => {
        console.error('錯誤獲取使用者位置', error);
      });
    } else {
      console.error('地理位置錯誤');
    }
  
    const scene = document.createElement('a-scene');
    scene.setAttribute('id', 'scene');
    scene.setAttribute('arjs', 'sourceType: webcam; videoTexture: true; debugUIEnabled: false');
    scene.setAttribute('renderer', 'antialias: true; alpha: true');
    scene.setAttribute('vr-mode-ui', 'enabled: false');
    scene.setAttribute('cursor', 'rayOrigin: mouse');
    
    const camera = document.createElement('a-camera');
    camera.setAttribute('id', 'camera');
    camera.setAttribute('gps-new-camera', '');
    scene.appendChild(camera);
  
    const createARObject = (id, latitude, longitude, modelUrl) => {
      const target = document.createElement('a-entity');
      target.setAttribute('id', id);
      target.setAttribute('gps-new-entity-place', `latitude: ${latitude}; longitude: ${longitude}`);
      target.setAttribute('scale', '4 4 4');
      target.setAttribute('visible', 'false');
      target.setAttribute('look-at', '[gps-mew-camera]');
  
      const modelContainer = document.createElement('a-entity');
      modelContainer.setAttribute('gltf-model', `url(${modelUrl})`);
      modelContainer.setAttribute('animation-mixer', '');
  
      target.appendChild(modelContainer);
      scene.appendChild(target);
    };
  
    createARObject('target1', 24.149687287189632, 120.68297905956476, 'https://raw.githubusercontent.com/Rayasd396kr/ar/main/bluenew.glb');
    createARObject('target2', 24.1514351979012, 120.68242524993458, 'https://raw.githubusercontent.com/Rayasd396kr/ar/main/redlandmark.glb');
    createARObject('target3', 24.149761545708383, 120.68367990956797, 'https://raw.githubusercontent.com/Rayasd396kr/ar/main/redlandmark.glb');
    createARObject('target4', 24.15046289505919, 120.68410126817396, 'https://raw.githubusercontent.com/Rayasd396kr/ar/main/redlandmark.glb');
    createARObject('target5', 24.150411337619133, 120.68297407245603, 'https://raw.githubusercontent.com/Rayasd396kr/ar/main/bluenew.glb');
    createARObject('target6', 24.15120942332591, 120.68399093382135, 'https://raw.githubusercontent.com/Rayasd396kr/ar/main/redlandmark.glb');
    createARObject('target7', 24.151279396565393, 120.6829962759951, 'https://raw.githubusercontent.com/Rayasd396kr/ar/main/redlandmark.glb');
    createARObject('target8', 24.15229293057009, 120.68384512885488, 'https://raw.githubusercontent.com/Rayasd396kr/ar/main/redlandmark.glb');

    create.value = (startPosition, endPosition) => {
      const distance = haversineDistance(startPosition.lat, startPosition.lng, endPosition[0], endPosition[1]);
        
    //   const intervalDistance = 10; 
  
    //   let accumulatedDistance = intervalDistance;
  
    //   while (accumulatedDistance < distance) {
    //     const ratio = accumulatedDistance / distance;
    //     const waypoint = {
    //       lat: startPosition.lat + ratio * (endPosition[0] - startPosition.lat),
    //       lng: startPosition.lng + ratio * (endPosition[1] - startPosition.lng)
    //     };
    //     console.log(waypoint);
    //     generateARObject(waypoint.lat, waypoint.lng);
    //     accumulatedDistance += intervalDistance;
    //   }
        if (distance > 30) {
            const ratio = 0.1; 
            const waypoint = {
                lat: startPosition.lat + ratio * (endPosition[0] - startPosition.lat),
                lng: startPosition.lng + ratio * (endPosition[1] - startPosition.lng)
            };
            const existingEntity = scene.querySelector('#direction');
            if (existingEntity) {
                scene.removeChild(existingEntity);
            }
            generateARObject(waypoint.lat, waypoint.lng);
        } else {
            if (arEntity) {
                arEntity.setAttribute('visible', 'false');
            }
        }
    };
    const generateARObject = (latitude, longitude) => {
    const arEntity = document.createElement('a-entity');
    arEntity.setAttribute('id', 'direction');
    arEntity.setAttribute('gps-new-entity-place', `latitude: ${latitude}; longitude: ${longitude}`);
    arEntity.setAttribute('scale', '4 4 4');
    arEntity.setAttribute('visible', 'true');
    arEntity.setAttribute('animation', 'property: scale; to: 1 1 1; dir: alternate; dur: 2000; loop: true;');
    arEntity.setAttribute('look-at', '[camera]');
    const arModelContainer = document.createElement('a-entity');
    arModelContainer.setAttribute('gltf-model', 'url(https://raw.githubusercontent.com/Rayasd396kr/ar/main/up.glb)');
    arModelContainer.setAttribute('animation-mixer', '');
  
    arEntity.appendChild(arModelContainer);
    scene.appendChild(arEntity);
  };
   
  
  
  
  
    const arContainer = document.getElementById('ar-container');
    arContainer.appendChild(scene);
  });
  
  // onBeforeUnmount(() => {
  //   if (watchId) {
  //     navigator.geolocation.clearWatch(watchId);
  //   }
  // });
  </script>
  
  <style scoped>
  #map {
      position: fixed;
      top: 10px; 
      left: 10px;
      width: 20vh;  
      height: 20vh;
      border-radius: 50%;
      overflow: hidden; 
      z-index: 1000;
  }

  #distance-display {
      position: fixed;
      top: calc(20vh + 20px); 
      left: 10px;
      width: 20vh;  
      color: aliceblue;
      background-color: rgba(48, 47, 47, 0.8);
      border-radius: 5px;
      z-index: 1001; 
      text-align: center; 
  }
  
  #side-menu {
    position: fixed;
    top: 0;
    right: 0%;
    width: 180px;
    height: 100%;
    background-color: rgb(244, 243, 243);
    color: white;
    overflow-x: hidden;
    transition: transform 0.5s ease, right 0.5s ease;
    transform: translateX(250px);
    z-index: 1000;
  }
  
  #side-menu.open {
    transform: translateX(0);
  }
  
  #menu-toggle {
    position: fixed;
    top: 8px;
    right: 10px;
    width: 40px;
    height: 40px;
    background-color: black;
    border: none;
    color: white;
    cursor: pointer;
    z-index: 1001;
    transition: transform 0.5s ease, right 0.5s ease;
    font-size: large;
  }
  
  .menu-open {
    transform: translateX(-450%);
  }

  #side-menu ul {
    list-style-type: none;
    padding: 0;
    margin: 0;
  }
  
  

  #side-menu ul li button {
    background-color: transparent;
    border: none;
  }
  
  .search-container {
  position: relative;
  display: flex; 
  justify-content: center; 
  align-items: center; 
}

.search-container input {
  padding: 5px 12px; 
  margin-top: 15px;
  border-radius: 3px; 
  border: 1px solid #ccc;
  width: 80%; 
  box-sizing: border-box; 
}

/* input {
  touch-action: manipulation; 
} */

.search-container .search-icon-button {
  position: absolute;
  right: 20px; 
  top: 67%; 
  transform: translateY(-50%); 
  background-color: transparent; 
  color: gray;
  border: none; 
  cursor: pointer; 
}

button .arrow {
  margin-left: 10px;
  transition: transform 0.5s ease;
}

button.open .arrow {
  transform: rotate(180deg);
}

#side-menu .toggle-container{
  padding: 0px 20px;
  text-align: left;
  color: black;
}

.toggle-dropdown{
  border-bottom: 1px solid rgb(183, 182, 182);
}
.toggle-dropdown button{
  font-size: 14px; 
  font-weight: bold; 
  padding: 10px;
  color: rgb(16, 83, 155);
}
#side-menu .s-dropdown{
  padding: 0px 10px 10px 10px;
  font-size: 12px; 
}

@keyframes slide-up {
  from {
    max-height: 1000px;
    opacity: 1;
  }
  to {
    max-height: 0;
    opacity: 0;
  }
}

@keyframes slide-down {
  from {
    max-height: 0;
    opacity: 0;
  }
  to {
    max-height: 1000px;
    opacity: 1;
  }
}

.slide-fade-enter-active {
  animation: slide-down 1s ease-in-out forwards;
}

.slide-fade-leave-active {
  animation: slide-up 0.7s ease-in-out forwards;
}
.serschtext{
  color: black;
  margin-top: 10px;
  margin-left: 30px;
}
  </style>
  
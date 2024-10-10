<template>
  <div>
    <div ref="threeContainer" style="width: 100%; height: 100vh; overflow: hidden;"></div>
    <div id="progress-container" v-if="loading">
      <p>Loading...</p>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue';
import { useRouter } from 'vue-router'; // Import useRouter
import * as THREE from 'three';
import { GLTFLoader } from 'three/addons/loaders/GLTFLoader.js';
import { OrbitControls } from 'three/addons/controls/OrbitControls.js';

const threeContainer = ref(null);
const loading = ref(true);
const router = useRouter(); // Initialize the router

// 定义 mouse 和 raycaster
let mouse = new THREE.Vector2();
let raycaster = new THREE.Raycaster();
let scene, camera, renderer, controls;

onMounted(() => {
  // 初始化 Three.js 渲染器
  renderer = new THREE.WebGLRenderer({ antialias: true });
  renderer.outputColorSpace = THREE.SRGBColorSpace;
  renderer.setSize(window.innerWidth, window.innerHeight);
  renderer.setClearColor(0xffffff);
  renderer.setPixelRatio(window.devicePixelRatio);
  renderer.shadowMap.enabled = true;
  renderer.shadowMap.type = THREE.PCFSoftShadowMap;

  threeContainer.value.appendChild(renderer.domElement);

  // 创建场景
  scene = new THREE.Scene();

  // 创建相机
  camera = new THREE.PerspectiveCamera(45, window.innerWidth / window.innerHeight, 1, 1000);
  camera.position.set(0, 2, 9);

  // 初始化 OrbitControls
  controls = new OrbitControls(camera, renderer.domElement);
  controls.enableDamping = true;
  controls.enablePan = false;
  controls.minDistance = 5;
  controls.maxDistance = 20;
  controls.minPolarAngle = 0.5;
  controls.maxPolarAngle = 1.5;
  controls.autoRotate = false;
  controls.target = new THREE.Vector3(0, 1, 0);
  controls.update();

  // 添加地板
  const groundGeometry = new THREE.PlaneGeometry(20, 20, 32, 32);
  groundGeometry.rotateX(-Math.PI / 2);
  const groundMaterial = new THREE.MeshStandardMaterial({
    color: 0x555555,
    side: THREE.DoubleSide
  });
  const groundMesh = new THREE.Mesh(groundGeometry, groundMaterial);
  groundMesh.castShadow = false;
  groundMesh.receiveShadow = true;
  scene.add(groundMesh);

  // 添加聚光灯
  const spotLight = new THREE.SpotLight(0xffffff, 3000, 100, Math.PI / 3, 1);
  spotLight.position.set(0, 25, 20); // 调整位置
  spotLight.angle = Math.PI / 3; // 增加角度范围
  spotLight.distance = 100; // 确保足够距离覆盖两个模型
  spotLight.castShadow = true;
  spotLight.shadow.bias = -0.0001;

  const lightTarget = new THREE.Object3D();
  lightTarget.position.set(1, 1.05, 1.5); // 两个模型之间的点
  scene.add(lightTarget);
  spotLight.target = lightTarget;
  scene.add(spotLight);

  // 加载模型
  const loader = new GLTFLoader().setPath('/Images/');
  // 第一个模型
  loader.load('zhongshang.glb', (gltf) => {
    const mesh1 = gltf.scene;
    mesh1.name = 'glbObject1';  // 设置名称
    mesh1.traverse((child) => {
      if (child.isMesh) {
        child.castShadow = true;
        child.receiveShadow = true;
      }
    });
    mesh1.scale.set(1.5, 1.5, 1.5);
    mesh1.position.set(0, 1.05, 3);
    mesh1.rotation.y = Math.PI / 1.5;

    scene.add(mesh1);
    loading.value = false;
  });

  // 第二个模型
  loader.load('zixun.glb', (gltf) => {
    const mesh2 = gltf.scene;
    mesh2.name = 'glbObject2';  // 设置名称
    mesh2.traverse((child) => {
      if (child.isMesh) {
        child.castShadow = true;
        child.receiveShadow = true;
      }
    });
    mesh2.scale.set(1.5, 1.5, 1.5);
    mesh2.position.set(1.3, 1.05, 1);
    mesh2.rotation.y = Math.PI / 4;

    scene.add(mesh2);
  });

  loader.load('jsdl.glb', (gltf) => {
    const mesh3 = gltf.scene;
    mesh3.name = 'glbObject2';  // 设置名称
    mesh3.traverse((child) => {
      if (child.isMesh) {
        child.castShadow = true;
        child.receiveShadow = true;
      }
    });
    mesh3.scale.set(0.5, 0.5, 0.5);
    mesh3.position.set(0, 3.05, 3);
    mesh3.rotation.y = Math.PI / 100;

    scene.add(mesh3);
  });

  loader.load('js.glb', (gltf) => {
    const mesh4 = gltf.scene;
    mesh4.name = 'glbObject2';  // 设置名称
    mesh4.traverse((child) => {
      if (child.isMesh) {
        child.castShadow = true;
        child.receiveShadow = true;
      }
    });
    mesh4.scale.set(0.5, 0.5, 0.5);
    mesh4.position.set(1.3, 2.55, 1);
    mesh4.rotation.y = Math.PI / 4;

    scene.add(mesh4);
  });

  // 添加事件监听器
  window.addEventListener('resize', onWindowResize);
  window.addEventListener('mousemove', onMouseMove);
  window.addEventListener('mousedown', onMouseDown);

  // 动画循环
  animate();
});

function onWindowResize() {
  camera.aspect = window.innerWidth / window.innerHeight;
  camera.updateProjectionMatrix();
  renderer.setSize(window.innerWidth, window.innerHeight);
}

function onMouseMove(event) {
  event.preventDefault();
  mouse.x = (event.clientX / window.innerWidth) * 2 - 1;
  mouse.y = -(event.clientY / window.innerHeight) * 2 + 1;
}

function onMouseDown(event) {
  event.preventDefault();
  mouse.x = (event.clientX / window.innerWidth) * 2 - 1;
  mouse.y = -(event.clientY / window.innerHeight) * 2 + 1;
  raycaster.setFromCamera(mouse, camera);
  const intersects = raycaster.intersectObjects(scene.children, true);
  if (intersects.length > 0) {
    const object = intersects[0].object;
    handleIntersections(object);
  }
}

function handleIntersections(object) {
  console.log('Clicked object:', object);
  console.log(`Clicked object name: ${object.name}`);

  switch (object.name) {
    case '最最最最最後了拉qaq_1':
      console.log('Navigating to floorselect');
      router.push({ name: 'floorselect1' });
      break;
    case '資訊樓_1':  // Ensure this name matches the actual name in your model
      console.log('Navigating to floorselect');
      router.push({ name: 'floorselect' });
      break;
    default:
      console.log('Intersects with an unknown object');
  }
}



function animate() {
  requestAnimationFrame(animate);
  renderer.render(scene, camera);
  controls.update();
}

onBeforeUnmount(() => {
  window.removeEventListener('resize', onWindowResize);
  window.removeEventListener('mousedown', onMouseDown);
  window.removeEventListener('mousemove', onMouseMove);
  renderer.dispose();
  controls.dispose();
});
</script>

<style scoped>
#progress-container {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  color: white;
  font-size: 1.5rem;
}
</style>

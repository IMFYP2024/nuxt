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
  import * as THREE from 'three';
  import { GLTFLoader } from 'three/addons/loaders/GLTFLoader.js';
  import { OrbitControls } from 'three/addons/controls/OrbitControls.js';
  
  const threeContainer = ref(null);
  const loading = ref(true);
  
  onMounted(() => {
    // 初始化 Three.js 渲染器
    const renderer = new THREE.WebGLRenderer({ antialias: true });
    renderer.outputColorSpace = THREE.SRGBColorSpace;
    renderer.setSize(window.innerWidth, window.innerHeight);
    renderer.setClearColor(0x000000);
    renderer.setPixelRatio(window.devicePixelRatio);
    renderer.shadowMap.enabled = true;
    renderer.shadowMap.type = THREE.PCFSoftShadowMap;
  
    // 将渲染器附加到模板中的 DOM 元素
    threeContainer.value.appendChild(renderer.domElement);
  
    // 创建场景
    const scene = new THREE.Scene();
  
    // 创建相机
    const camera = new THREE.PerspectiveCamera(45, window.innerWidth / window.innerHeight, 1, 1000);
    camera.position.set(0,2, 11);
  
    // 初始化 OrbitControls
    const controls = new OrbitControls(camera, renderer.domElement);
    controls.enableDamping = true;
    controls.enablePan = false;
    controls.minDistance = 5;
    controls.maxDistance = 20;
    controls.minPolarAngle = 0.5;
    controls.maxPolarAngle = 1.5;
    controls.autoRotate = false;
    controls.target = new THREE.Vector3(0 , 1, 0);
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
    const spotLight = new THREE.SpotLight(0xffffff, 3000, 100, 0.22, 1);
    spotLight.position.set(0, 25, 10);
    spotLight.castShadow = true;
    spotLight.shadow.bias = -0.0001;
    scene.add(spotLight);
  
    // 加载模型
    const loader = new GLTFLoader().setPath('/Images/');
    // 第一个模型
    loader.load('zhongshang.glb', (gltf) => {
      const mesh1 = gltf.scene;

      mesh1.traverse((child) => {
        if (child.isMesh) {
          child.castShadow = true;
          child.receiveShadow = true;
        }
      });

      mesh1.position.set(0, 1.05, 3);
      mesh1.rotation.y = Math.PI / 1.5;  // 绕 X 轴旋转 90 度
      // 设置模型的初始旋转

      scene.add(mesh1);

      // 隐藏加载进度条
      loading.value = false;
    }, (xhr) => {
      console.log(`Loading: ${(xhr.loaded / xhr.total * 100).toFixed(2)}%`);
    }, (error) => {
      console.error('An error happened', error);
    });

    // 第二个模型
    loader.load('zixun.glb', (gltf) => {
      const mesh2 = gltf.scene;

      mesh2.traverse((child) => {
        if (child.isMesh) {
          child.castShadow = true;
          child.receiveShadow = true;
        }
      });

      // 设置第二个模型的位置和旋转
      mesh2.position.set(0.25, 2.5, 3);  // 不同的位置
      // mesh2.rotation.y = Math.PI / 2;  // 绕 X 轴旋转 90 度

      scene.add(mesh2);
    }, (xhr) => {
      console.log(`Loading another model: ${(xhr.loaded / xhr.total * 100).toFixed(2)}%`);
    }, (error) => {
      console.error('An error happened with another model', error);
    });

  // 假设 mesh 是你加载的 3D 模型
  // mesh.rotation.x = Math.PI / 2;  // 绕 X 轴旋转 90 度
  // mesh.rotation.y = Math.PI / 4;  // 绕 Y 轴旋转 45 度
  // mesh.rotation.z = Math.PI / 6;  // 绕 Z 轴旋转 30 度

    // 处理窗口大小变化
    window.addEventListener('resize', onWindowResize);
  
    function onWindowResize() {
      camera.aspect = window.innerWidth / window.innerHeight;
      camera.updateProjectionMatrix();
      renderer.setSize(window.innerWidth, window.innerHeight);
    }
  
    // 动画循环
    function animate() {
      requestAnimationFrame(animate);
      controls.update();
      renderer.render(scene, camera);
    }
  
    animate();
  
    // 清理事件监听器和 Three.js 实例
    onBeforeUnmount(() => {
      window.removeEventListener('resize', onWindowResize);
      renderer.dispose();
      controls.dispose();
    });
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
  
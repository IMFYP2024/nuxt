<template>
  <div class="container1">
      <div ref="threeContainer" class="three-container"></div>
      <h1>中商大樓</h1>
      <h2>資訊舘</h2>
  </div>
</template>

<script>
import * as THREE from 'three';
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls.js';
import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader.js';

export default {
  name: 'ThreeScene',
  mounted() {
    this.initThree();
  },
  
  methods: {
    initThree() {
      this.scene = new THREE.Scene();
      this.camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 100);
      this.renderer = new THREE.WebGLRenderer({ alpha: true });
      this.renderer.setSize(window.innerWidth, window.innerHeight);
      this.renderer.setClearColor(0x000000, 0); // 透明背景
      this.$refs.threeContainer.appendChild(this.renderer.domElement);

      this.controls = new OrbitControls(this.camera, this.renderer.domElement);
      this.controls.enableRotate = false;  // 禁用旋轉
      this.controls.enableZoom = false;    // 禁用縮放
      this.controls.enablePan = false;     // 禁用平移
      this.controls.update();

      const loader = new GLTFLoader();
      this.loadGLBModels(loader);

      this.addLights();

      this.camera.position.set(0, 1, 7.5);

      this.raycaster = new THREE.Raycaster();
      this.mouse = new THREE.Vector2();

      window.addEventListener('resize', this.onWindowResize, false);
      

      this.animate();
    },
    loadGLBModels(loader) {
      loader.load('/Images/zhongshang.glb', (glb) => {
             this.addObjectToScene(glb.scene, { x: 0, y: 3.5, z:-6 }, 4,-0.1, 'fontObject'); // 增加大小，並移動到 x = -2, z = -3
      });
      loader.load('/Images/zixun.glb', (glb) => {
              this.addObjectToScene(glb.scene, { x: 0.8, y: -1.3, z: 2 }, 3,-0.1, 'glbObject1'); // 增加大小，並移動到 x = 2, z = 2
      });
       },

    addObjectToScene(object, position, scale, rotationX, name) {
      object.position.set(position.x, position.y, position.z);
      object.scale.set(scale, scale, scale);
      object.rotation.x = rotationX;
      object.traverse(child => {
        if (child.isMesh) child.name = name;
      });
      this.scene.add(object);
    },
    
    addLights() {
      const light = new THREE.DirectionalLight(0xffffff, 3);
      light.position.set(5, 6, 14);
      this.scene.add(light);
    },
    onWindowResize() {
      this.camera.aspect = window.innerWidth / window.innerHeight;
      this.camera.updateProjectionMatrix();
      this.renderer.setSize(window.innerWidth, window.innerHeight);
    },
    
    
    animate() {
      this.animationId = requestAnimationFrame(this.animate);
      // this.scene.traverse((object) => {
      //     if (object.isMesh) {
      //     object.rotation.z+= 0.01; // 在 Y 軸上旋轉
      //     }
      // });
      this.renderer.render(this.scene, this.camera);
      this.controls.update();
    },
    cleanUpThree() {
      cancelAnimationFrame(this.animationId);
      window.removeEventListener('resize', this.onWindowResize);
      while (this.scene.children.length > 0) {
        const object = this.scene.children[0];
        this.scene.remove(object);
        if (object.geometry) object.geometry.dispose();
        if (object.material) {
          if (Array.isArray(object.material)) {
            object.material.forEach(material => material.dispose());
          } else {
            object.material.dispose();
          }
        }
      }
      this.renderer.domElement.remove();
      this.renderer.dispose();
      this.scene = null;
      this.camera = null;
      this.controls = null;
      this.raycaster = null;
      this.mouse = null;
    },
    
  }
};

definePageMeta({
  layout: false
});
</script>


<style scoped>
.container1{
  display: flex;
  flex-direction: column;
  align-items: center;
}
.container1 h1{
  margin-top: 20%;
  font-size: 36px;
}
  .container1 h2{
  font-size: 36px;
  margin-top:60% ;
}
.three-container {
  position: absolute;
  top: 15%;
  left: 0;
  width: 100%;
  height: 100vh; 
  overflow: hidden;
}
</style>

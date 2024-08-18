<template>
  <div class="background-container">
    <div ref="threeContainer" class="three-container"></div>
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
  beforeDestroy() {
    this.cleanUpThree();
    window.removeEventListener('resize', this.onWindowResize);
    window.removeEventListener('mousedown', this.onMouseDown);
    window.removeEventListener('mousemove', this.onMouseMove);
  },
  methods: {
    initThree() {
      this.scene = new THREE.Scene();
      this.camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
      this.renderer = new THREE.WebGLRenderer({ alpha: true }); // 使用透明背景
      this.renderer.setSize(window.innerWidth, window.innerHeight);
      this.$refs.threeContainer.appendChild(this.renderer.domElement);

      this.controls = new OrbitControls(this.camera, this.renderer.domElement);
      this.controls.update();
      this.controls.enableRotate = false;
      this.controls.enableZoom = false;
      const loader = new GLTFLoader();
      this.loadGLBModels(loader);

      this.addLights();

      this.camera.position.set(0, 0, 7);
      console.log(this.camera.position);

      this.raycaster = new THREE.Raycaster();
      this.mouse = new THREE.Vector2();

      window.addEventListener('resize', this.onWindowResize, false);
      window.addEventListener('mousedown', this.onMouseDown, false);
      window.addEventListener('mousemove', this.onMouseMove, false);

      this.animate();
    },
    loadGLBModels(loader) {
      loader.load('/Images/fly_c.glb', (glb) => {
        console.log('GLB Loaded:', glb.scene);
        this.addObjectToScene(glb.scene, { x: 0, y: 4.5, z: -1 }, 1, 0, 'fontObject');
      });
      loader.load('/Images/Ar_darkblue.glb', (glb) => {
        console.log('GLB Loaded:', glb.scene);
        this.addObjectToScene(glb.scene, { x: 0, y: 2.2, z: 0 }, 0.5, Math.PI / 2, 'glbObject1');
      });
      loader.load('/Images/introducedark1.glb', (glb) => {
        console.log('GLB Loaded:', glb.scene);
        this.addObjectToScene(glb.scene, { x: 0, y: 0.7, z: 0 }, 0.5, Math.PI / 2, 'glbObject2');
      });
      loader.load('/Images/pointcard_darkblue.glb', (glb) => {
        console.log('GLB Loaded:', glb.scene);
        this.addObjectToScene(glb.scene, { x: 0, y: -0.8, z: 0 }, 0.5, Math.PI / 2, 'glbObject3');
      });
      loader.load('/Images/pointcard_darkblue.glb', (glb) => {
        console.log('GLB Loaded:', glb.scene);
        this.addObjectToScene(glb.scene, { x: 0, y: -2.3, z: 0 }, 0.5, Math.PI / 2, 'glbObject4');
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
    onMouseMove(event) {
      event.preventDefault();
      this.mouse.x = (event.clientX / window.innerWidth) * 2 - 1;
      this.mouse.y = -(event.clientY / window.innerHeight) * 2 + 1;
    },
    onMouseDown(event) {
      event.preventDefault();
      this.mouse.x = (event.clientX / window.innerWidth) * 2 - 1;
      this.mouse.y = -(event.clientY / window.innerHeight) * 2 + 1;
      this.raycaster.setFromCamera(this.mouse, this.camera);
      const intersects = this.raycaster.intersectObjects(this.scene.children, true);
      if (intersects.length > 0) {
        const object = intersects[0].object;
        this.handleIntersections(object);
      }
    },
    handleIntersections(object) {
      console.log(`Clicked object name: ${object.name}`);
      switch (object.name) {
        case 'glbObject1':
          this.$router.push({ name: 'mindar' }).then(() => this.cleanUpThree());
          break;
        case 'glbObject2':
          this.$router.push({ name: 'deintroduce' }).then(() => this.cleanUpThree());
          break;
        case 'glbObject3':
          this.$router.push({ name: 'point' }).then(() => this.cleanUpThree());
          break;
        case 'glbObject4':
          this.$router.push({ name: 'about' }).then(() => this.cleanUpThree());
          break;
        case 'fontObject':
          console.log('Clicked font object');
          break;
        default:
          console.log('Intersects with an unknown object');
      }
    },
    animate() {
      this.animationId = requestAnimationFrame(this.animate);
      this.renderer.render(this.scene, this.camera);
      console.log('Scene rendered');
      this.controls.update();
    },
    cleanUpThree() {
      cancelAnimationFrame(this.animationId);
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
    }
  }
};
</script>

<script setup>
definePageMeta({
  layout: 'default'
})
</script>

<style>
html, body {
  margin: 0;
  padding: 0;
  width: 100%;
  height: 100%;
  overflow: hidden;
}

.background-container {
  width: 100%;
  height: 100%;
  background: url('/videos/IMG_0184.mov') no-repeat center center fixed;
  background-size: cover;
  position: relative;
}

.three-container {
  width: 100%;
  height: 100%;
  position: absolute;
  top: 0;
  left: 0;
  background-color: rgba(255, 0, 0, 0.5); /* 暂时加一个半透明红色背景 */
}
</style>

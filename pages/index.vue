<template>
  <div>
    <video autoplay muted loop playsinline class="background-video">
      <source src="/videos/IMG_0184.mov" type="video/mp4">
    </video>
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
      this.renderer = new THREE.WebGLRenderer({ alpha: true });
      this.renderer.setSize(window.innerWidth, window.innerHeight);
      this.renderer.setClearColor(0x000000, 0); // 透明背景
      this.$refs.threeContainer.appendChild(this.renderer.domElement);

      this.controls = new OrbitControls(this.camera, this.renderer.domElement);
      this.controls.update();

      const loader = new GLTFLoader();
      this.loadGLBModels(loader);

      this.addLights();

      this.camera.position.set(0, 0, 7);

      this.raycaster = new THREE.Raycaster();
      this.mouse = new THREE.Vector2();

      window.addEventListener('resize', this.onWindowResize, false);
      window.addEventListener('mousedown', this.onMouseDown, false);
      window.addEventListener('mousemove', this.onMouseMove, false);

      this.animate();
    },
    loadGLBModels(loader) {
      loader.load('/Images/fly_c.glb', (glb) => {
        this.addObjectToScene(glb.scene, { x: 0, y: 4, z: -1 }, 1, 0, 'fontObject');
      });
      loader.load('/Images/Ar_darkblue.glb', (glb) => {
        this.addObjectToScene(glb.scene, { x: 0, y: 1.5, z: 0 }, 0.5, Math.PI / 2, 'glbObject1');
      });
      loader.load('/Images/introducedark1.glb', (glb) => {
        this.addObjectToScene(glb.scene, { x: 0, y: 0, z: 0 }, 0.5, Math.PI / 2, 'glbObject2');
      });
      loader.load('/Images/about.glb', (glb) => {
        this.addObjectToScene(glb.scene, { x: 0, y: -1.5, z: 0 }, 0.5, Math.PI / 2, 'glbObject3');
      });
      /*loader.load('/Images/pointcard_darkblue.glb', (glb) => {
        this.addObjectToScene(glb.scene, { x: 0, y: -2.3, z: 0 }, 0.5, Math.PI / 2, 'glbObject4');
      });*/
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
    /*setupVideo() {
      const video = document.createElement('video');
      video.src = '/videos/IMG_0184.mov';
      video.crossOrigin = 'anonymous';
      video.loop = true;
      video.muted = true;
      video.load();
      video.play();

      const videoTexture = new THREE.VideoTexture(video);
      const geometry = new THREE.BoxGeometry(7.5, 16, 2);
      const material = new THREE.MeshBasicMaterial({ map: videoTexture });
      const videoBox = new THREE.Mesh(geometry, material);
      videoBox.position.set(0, 0, -4.4);
      videoBox.name = 'videoBox';
      return videoBox;
    },
    */
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
          this.$router.push({ name: 'about' }).then(() => this.cleanUpThree());
          break;
        /*case 'glbObject4':
          this.$router.push({ name: 'point' }).then(() => this.cleanUpThree());
          break;*/
        case 'fontObject':
          console.log('Clicked font object');
          break;
        case 'videoBox':
          console.log('Clicked video box');
          break;
        default:
          console.log('Intersects with an unknown object');
      }
    },
    animate() {
      this.animationId = requestAnimationFrame(this.animate);
      this.renderer.render(this.scene, this.camera);
      this.controls.update();
    },
    cleanUpThree() {
      cancelAnimationFrame(this.animationId);
      window.removeEventListener('resize', this.onWindowResize);
      window.removeEventListener('mousedown', this.onMouseDown);
      window.removeEventListener('mousemove', this.onMouseMove);
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


<style>
html, body {
  margin: 0;
  padding: 0;
  width: 100%;
  height: 100%;
  overflow: hidden;
  position: relative;
}
.background-video {
  position: absolute;
  top: 0;
  left: 0;
  height: 100%!important; 
  width: 100%;
  object-fit: cover;
  z-index: -2; 
}
.three-container {
  position: relative;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: -1; 
  overflow: hidden;
}
</style>

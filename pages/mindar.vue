<template>
    <div class="video-overlay">
      <iframe id="iframe" ref="iframeRef" src="https://rayasd396kr.github.io/mindar" allow="fullscreen;camera;geolocation; gyroscope; accelerometer;magnetometer"></iframe> 
    </div>
</template>
<script>
export default {
  mounted() {
    window.addEventListener('message', this.handleMessage);
  },
  beforeDestroy() {
    window.removeEventListener('message', this.handleMessage);
  },
  methods: {
    handleMessage(event) {
      if (event.data.action === 'navigate' && event.data.url) {
        this.$router.push(event.data.url);
      }
    }
  }
};
</script>
<style scoped>
    iframe {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    border: none;
    }
    
</style>
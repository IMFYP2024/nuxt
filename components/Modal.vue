<template>
  <div v-if="showModal" class="modal-overlay" @click="handleOverlayClick">
    <div class="modal-content" @click.stop>
      <div class="modal-header">
        <h2>{{ title }}</h2>
        <button class="modal-close" @click="close">×</button>
      </div>
      <div class="modal-body">
        <slot name="itemText"></slot>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    title: {
      type: String,
      default: 'Modal Title'
    },
    showModal: {
      type: Boolean,
      default: false
    }
  },
  methods: {
    close() {
      this.$emit('closing');
    },
    handleOverlayClick() {
      this.close();
    }
  }
}
</script>

<style scoped>
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1000;
  animation: fadeInOverlay 0.3s ease-in-out;
}

.modal-content {
  background: white;
  border-radius: 8px;
  overflow: hidden;
  width: 90%;
  max-width: 500px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
  animation: fadeInContent 0.3s ease-in-out;
}

.modal-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 16px;
  background: #f1f1f1;
  border-bottom: 1px solid #ddd;
}

.modal-header h2 {
  margin: 0;
}

.modal-close {
  background: none;
  border: none;
  font-size: 24px;
  cursor: pointer;
}

.modal-body {
  padding: 16px;
}

@keyframes fadeInOverlay {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}

@keyframes fadeInContent {
  from {
    opacity: 0;
    transform: translateY(-50px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.fade-leave-active {
  animation: fadeOut 0.3s ease-in-out;
}

@keyframes fadeOut {
  from {
    opacity: 1;
  }
  to {
    opacity: 0;
  }
}
</style>

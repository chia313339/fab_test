<template>
  <div class="fullwidthbanner-container">
    <div class="tag-box">
      <div class="tag">
        <b>空間奇境 &nbsp;&nbsp;&nbsp; | &nbsp;&nbsp;&nbsp; <span>平面規劃</span></b>
      </div>
    </div>
    <div class="content">
      <div class="image-container">
        <transition name="fade" mode="out-in">
          <img :src="currentImage" alt="" class="bg" :key="currentImage">
        </transition>
      </div>
      <div class="floor">
        <span 
          v-for="floor in floors" 
          :key="floor" 
          :class="{ active: selectedFloor === floor }" 
          @click="selectFloor(floor)"
        >
          {{ floor }}
        </span>
      </div>
    </div>
  </div>
</template>

<script>
import { onMounted, nextTick, ref } from 'vue';


export default {
  setup() {
    const selectedFloor = ref('3F');
    const currentImage = ref('/img/p43/3f.png');
    const floors = Array.from({ length: 20 }, (_, i) => `${i + 3}F`);

    const selectFloor = (floor) => {
      selectedFloor.value = floor;
      currentImage.value = `/img/p43/${floor.toLowerCase()}.png`;
    };

    const initContent = () => {
      nextTick(() => {
        const content = document.querySelector('.content');
        if (content) {
          content.style.opacity = 0;
          setTimeout(() => {
            content.style.opacity = 1;
          }, 100);
        }
      });
    };

    

    onMounted(initContent);

    return {
      floors,
      selectedFloor,
      currentImage,
      selectFloor,
    };
  }
};
</script>

<style scoped>
.content {
  display: flex;
  flex-direction: column;
  align-items: center; /* 垂直居中 */
  justify-content: center; /* 水平居中 */
  position: relative; /* 使內部元素可以使用絕對定位 */
  overflow: hidden; /* 防止图片超出容器 */
  opacity: 0; /* 初始状态透明 */
  transition: opacity 2s ease-in-out; /* 淡入效果 */
  width: 90%;
  height: auto; /* 這裡你可以根據需要調整高度 */
  z-index: -1; /* 確保背景圖片在最下層 */
}

.image-container {
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
}

.bg {
  max-width: 100%; /* 图片最大宽度为容器宽度 */
  max-height: 100%; /* 图片最大高度为容器高度 */
  object-fit: contain; /* 保持图片的纵横比，确保图片完整呈现 */
}

.floor {
  position: absolute;
  bottom: 0;
  width: 100%;
  display: flex;
  justify-content: center;
  gap: 5px; /* span之間的間隔 */
  padding-bottom: 10%;
  background-color: rgba(255, 255, 255, 0.8); /* 半透明背景 */
}

.floor span {
  padding: 5px 10px;
  font-weight: bold;
  color: black;
  background-color: white;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s, color 0.3s;
}

.floor span.active {
  color: white;
  background-color: #5B4341;
}

/* 新增淡入效果的 transition */
.fade-enter-active, .fade-leave-active {
  transition: opacity 1s ease-in-out;
}
.fade-enter-from, .fade-leave-to {
  opacity: 0;
}
</style>

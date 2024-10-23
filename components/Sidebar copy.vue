<template>
  <aside :class="['sidebar', { 'is-mobile': isMobile }]" v-if="showSidebar">
    <div class="sidebar-top">
      <NewChatButton />
    </div>
    <div class="sidebar-middle">
      <HistoryContainer />
    </div>
    <div class="sidebar-bottom">
      <SettingButton />
    </div>
  </aside>
  <div class="overlay" v-if="showSidebar && isMobile" @click="closeSidebar"></div> <!-- 遮罩层 -->
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue';
import NewChatButton from './NewChatButton.vue';
import HistoryContainer from './HistoryContainer.vue';
import SettingButton from './SettingButton.vue';
import { useSidebarStore } from '~/store/sidebar';

const showSidebar = ref(true);  // 控制侧边栏显示

const isMobile = useMediaQuery('(max-width: 768px)');
const sidebarStore = useSidebarStore();
const closeSidebar = () => {
  showSidebar.value = false;  // 点击遮罩层关闭侧边栏
};

const handleResize = () => {
  isMobile.value = window.innerWidth <= 768;
};

onMounted(() => {
  handleResize();  // 初始化时检测是否为移动端
  window.addEventListener('resize', handleResize);  // 监听窗口大小变化
});

onBeforeUnmount(() => {
  window.removeEventListener('resize', handleResize);  // 清除监听
});
</script>

<style scoped>
.sidebar {
  display: flex;
  flex-direction: column;
  height: 100%;
  width: 260px;
  min-width: 200px;
  background-color: #f4f4f4;
  z-index: 10; /* 确保在上层 */
  transition: transform 0.3s ease;
}

.sidebar.is-mobile {
  position: fixed;
  top: 0;
  left: 0;
  height: 98vh;
  width: 80%;
  max-width: 240px;
  z-index: 100; /* 更高的层级确保在最上方 */
  transform: translateX(0); /* 移动端时从左侧滑出 */
}

.sidebar-top {
  flex: 0 0 auto;
}

.sidebar-middle {
  flex: 1 1 auto;
  overflow-y: auto;
}

.sidebar-bottom {
  flex: 0 0 auto;
}

/* 遮罩层，用于阻止与后面内容交互 */
.overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  background-color: rgba(0, 0, 0, 0.2); /* 半透明黑色遮罩 */
  z-index: 99; /* 层级比侧边栏低 */
}

@media (max-width: 768px) {
  .sidebar {
    transform: translateX(-100%);
  }
}
</style>
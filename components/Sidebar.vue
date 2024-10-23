<template>
  <!-- 侧边栏 -->
  <aside :class="['sidebar', { 'is-mobile': isMobile }]" v-if="sidebarVisible">
    <div class="sidebar-top">
      <!-- 可以放置新聊天按钮 -->
      <NewChatButton />
    </div>
    <div class="sidebar-middle">
      <!-- 历史记录容器 -->
      <HistoryContainer />
    </div>
    <div class="sidebar-bottom">
      <!-- 设置按钮 -->
      <SettingButton />
    </div>
  </aside>

  <!-- 当在移动端并且侧边栏显示时，显示遮罩层，点击遮罩层关闭侧边栏 -->
  <div class="overlay" v-if="isMobile && sidebarVisible" @click="closeSidebar"></div>
</template>

<script setup>
import { computed } from 'vue';
import { useMediaQuery } from '@vueuse/core';
import NewChatButton from './NewChatButton.vue';
import HistoryContainer from './HistoryContainer.vue';
import SettingButton from './SettingButton.vue';
import { useSidebarStore } from '~/store/sidebar';

// 引入 Pinia store 来管理侧边栏状态
const sidebarStore = useSidebarStore();

// 使用计算属性从 Pinia 中获取侧边栏的显示状态
const sidebarVisible = computed(() => sidebarStore.isVisible);

// 检测是否是移动端
const isMobile = useMediaQuery('(max-width: 768px)');

// 关闭侧边栏的方法
const closeSidebar = () => {
  sidebarStore.hideSidebar();
};

</script>

<style scoped>
.sidebar {
  display: flex;
  flex-direction: column;
  height: 100%;
  width: 260px;
  min-width: 200px;
  background-color: #f4f4f4;
  z-index: 10;
  transition: transform 0.3s ease;
}

.sidebar.is-mobile {
  position: fixed;
  top: 0;
  left: 0;
  height: 100vh;
  width: 80%;
  max-width: 240px;
  z-index: 100;
  transform: translateX(0);
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

/* 遮罩层 */
.overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  background-color: rgba(0, 0, 0, 0.2);
  z-index: 99;
}
</style>
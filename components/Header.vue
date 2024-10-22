<!-- components/Header.vue -->
<template>
  <header class="header">
    <div v-if="!isMobile" class="desktop-header">
      <Logo />
      <Menu />
    </div>
    <div v-else class="mobile-header">
      <button @click="toggleSidebar" class="menu-button">☰</button>
      <SelectModel />
      <NewChatButton />
    </div>
  </header>
</template>

<script setup>
import { useMediaQuery } from '@vueuse/core';
import Logo from './Logo.vue';
import Menu from './Menu.vue';
import NewChatButton from './NewChatButton.vue';
import SelectModel from './SelectModel.vue';
import { useSidebarStore } from '~/store/sidebar';

const isMobile = useMediaQuery('(max-width: 768px)');
const sidebarStore = useSidebarStore();

// 切换侧边栏显示状态
function toggleSidebar() {
  sidebarStore.isVisible = !sidebarStore.isVisible;
}
</script>

<style scoped>
.header {
  padding: 10px;
  display: flex;
  align-items: center;
  background-color: #ffffff;
  color: black;
  /*上圆角*/
  border-top-left-radius: 15px;
  border-top-right-radius: 15px;
}

.menu-button {
  background: none;
  border: none;
  cursor: pointer;
  font-size: 24px;
  color: #333;
}

.mobile-header {
  width: 100%;
  display: flex;
  flex: 1;
  justify-content: space-between;
  align-items: center;
  background-color: #ffffff;
  color: white;
  
}

.desktop-header  {
  display: flex;
  flex: auto;
  align-items: center;
}


</style>

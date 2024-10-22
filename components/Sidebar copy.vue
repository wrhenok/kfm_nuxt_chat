<!-- components/Sidebar.vue -->
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
</template>

<script setup>
import NewChatButton from './NewChatButton.vue';
import HistoryContainer from './HistoryContainer.vue';
import SettingButton from './SettingButton.vue';

const showSidebar = ref(true);  // 控制侧边栏显示
const isMobile = ref(false);

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
  width: 300px;
  min-width: 200px;
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

@media (max-width: 768px) {
  .sidebar-top{
    display: none;
  }
}
</style>

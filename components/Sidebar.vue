<!-- components/Sidebar.vue -->
<template>
  <aside class="sidebar">
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
</script>

<style scoped>
.sidebar {
  display: flex;
  flex-direction: column;
  height: 100%;
  width: 20%;
  min-width: 150px;
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

<!-- components/ChatList.vue -->
<template>
  <ul class="chat-list">
    <li v-for="chat in chats" :key="chat.id"
        class="chat-item"
        @click="selectChat(chat.id)">
      {{ chat.title }}
    </li>
  </ul>
</template>

<script setup>
import { ref } from 'vue';

const chats = ref([
  // 示例数据
  { id: 'abc5', title: 'abc5' },
  { id: 2, title: '对话 2' },
  { id: 3, title: '对话 3' },
  { id: 'abc5', title: 'abc5' },
  { id: 2, title: '对话 2' },
  { id: 3, title: '对话 3' },
  { id: 'abc5', title: 'abc5' },

]);
import { useCurrentPageStore } from '~/store/currentPage';

const currentPageStore = useCurrentPageStore();

import { useCurrentSessionStore } from '~/store/currentSession';
const currentSessionStore = useCurrentSessionStore();

function selectChat(id) {
  currentPageStore.setCurrentPage('history');
  currentSessionStore.setSessionId(id)

}
</script>
<!-- <style>
ul{
  cursor: pointer; /* 鼠标悬停时变成手型指针 */

}

li:hover {
  color: #2A70C2; /* 更深的蓝色 */
  background-color: #D6E5FF; /* 更深的背景色 */
}

li:hover::after {
  border-color: #2A70C2;
}
</style> -->

<style scoped>
.chat-list {
  list-style-type: none; /* 去掉默认的列表符号 */
  padding: 0;
  margin: 0;
  /* min-width: 40px; */
}

.chat-item {
  padding: 12px 16px;
  margin: 12px 10px;
  background-color: #ffffff; /* 设置列表项的背景色 */
  border-radius: 8px; /* 圆角边框 */
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.05); /* 添加轻微阴影 */
  cursor: pointer; /* 鼠标变为手形，表明可以点击 */
  transition: background-color 0.3s, box-shadow 0.3s; /* 添加过渡效果 */
  font-size: 14px;
  color: #333; /* 文字颜色 */
}

.chat-item:hover {
  background-color: #93c5fd; /* 悬停时的背景色变化 */
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1); /* 悬停时阴影变化 */
}

.chat-item:active {
  background-color: #93c5fd; /* 点击时的背景色 */
}

@media (max-width: 768px) {
  .chat-item {
    padding: 8px 12px;
    margin: 8px 8px;
    min-width: 30px;
  }
}
</style>
<!-- components/NewChatButton.vue -->
<template>
  <div class="new-chat">
    <button @click="startNewChat" class="new-chat-button">
      ➕ 新对话
    </button>
  </div>

</template>

<script setup>
import { useCurrentPageStore } from '~/store/currentPage';

const currentPageStore = useCurrentPageStore();

function startNewChat() {
  currentPageStore.setCurrentPage('newchat');
}
</script>

<style scoped>
.new-chat {
  /* width: 100%; */
  display: flex;
  justify-content: center;
}

.new-chat-button {
  width: 100%;
  padding: 12px;
  margin-bottom: 16px;
  margin: 10px;
  background-color: #007bff; /* 蓝色 */
  color: white;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  text-align: center;
  font-size: 16px;
  transition: background-color 0.3s ease;
}

.new-chat-button:hover {
  background-color: #2563eb; /* 更深的蓝色 */
}

.new-chat-button:focus {
  outline: none;
  box-shadow: 0 0 0 4px rgba(59, 130, 246, 0.5); /* 聚焦效果，蓝色外圈 */
}
</style>
<!-- components/UserInput.vue -->
<template>
  <div class="chat-container">
    <div class="chat-box">
      <div class="user-message">
        {{ content }}
      </div>
    </div>
    <div class="avatar">
      {{ name.charAt(0).toUpperCase() }}
    </div>
  </div>
</template>

<script setup>
const name = ref('开发喵');
defineProps({
  content: String,
});
</script>

<style scoped>
.chat-container {
  display: flex;
  justify-content: flex-end;
  padding: 20px 0;
}

.chat-box {
  display: flex;
  align-items: flex-start;
  max-width: 60%;
  border-radius: 10px;
  padding: 0 10px;
  position: relative;
}

.avatar {
  width: 40px;
  height: 40px;
  background-color: #3b82f6;
  color: #fff;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 16px;
  border-radius: 50%;
  margin-right: 10px;
}

.user-message {
  background-color: #3b82f6;
  border-radius: 10px;
  padding: 10px;
  max-width: 100%;
  font-size: 16px;
  color: #fff;
  box-shadow: 0px 2px 8px rgba(0, 0, 0, 0.1);
}

@media (max-width: 768px) {
  .avatar {
    display: none;
  }
}

</style>

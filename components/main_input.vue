<!-- components/main_input.vue -->
<template>
  <div class="main-input">
    <!-- 如果需要保留 select-list，可以放在输入框上方或旁边 -->
    <!--    -->
    <!-- 输入区域 -->
    <div class="input-container">
      <select-list />
      <textarea
          v-model="userInput"
          placeholder="发送消息"
          @input="adjustTextareaHeight"
          @keydown.enter.exact.prevent="sendMessage"
          ref="textarea"
      ></textarea>
      <button class="send-button" @click="sendMessage" :disabled="!userInput.trim()">
        <img src="../assets/icons/send-message.svg" alt="Send" />
      </button>
    </div>
  </div>
</template>


<script setup>
import { ref, computed, onBeforeUnmount } from 'vue'
import { useSelectModelStore } from '@/store/selectModel.js'
import { useSelectRagStore } from '@/store/selectRag.js'
import { useSelectListStore } from '@/store/selectList.js'
import { useMessageSessionStore } from '@/store/messageSession.js'
import SelectList from './SelectList.vue'

const userInput = ref('hi') // 设置默认值
const selectModelStore = useSelectModelStore()
const selectRagStore = useSelectRagStore()
const selectListStore = useSelectListStore()
const messageSession = useMessageSessionStore()

const evtSource = ref(null)

onBeforeUnmount(() => {
  if (evtSource.value) {
    evtSource.value.close()
  }
})

const sendMessage = () => {
  const message = userInput.value.trim()
  if (!message) return

  const model = selectModelStore.selectedModel
  const rag = selectRagStore.selectedRag
  const listItem = selectListStore.selectedItem

  if (evtSource.value) {
    evtSource.value.close()
  }

  // 添加用户消息
  messageSession.sessions.push({ role: 'user', content: message })

  // 清空输入框
  userInput.value = ''

  const encodedMessage = encodeURIComponent(message)
  const url = `http://127.0.0.1:8000/chat_stream/${encodedMessage}?model=${model}&rag=${rag}&listItem=${listItem}`

  // 使用 reactive 包裹 assistantMessage
  const assistantMessage = reactive({ role: 'assistant', content: '' })
  messageSession.sessions.push(assistantMessage)

  evtSource.value = new EventSource(url)

  evtSource.value.onmessage = (event) => {
    let data =  event.data;
    data = data.substring(1, event.data.length - 1)
    data = data.replace('\\n\\n', '\n')
    data = data.replace('\\n', '\n')
    console.log(data);
    assistantMessage.content += data;
  }

  evtSource.value.onerror = (err) => {
    console.error('EventSource failed:', err)
    evtSource.value.close()
    evtSource.value = null
  }
}

</script>

<style scoped>

.input-container {
  position: sticky; /* 使用 sticky 而不是 fixed */
  display: flex;
  align-items: center;
  background-color: #ececec;
  padding: 10px;
  border-radius: 70px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.05); /* 输入框的阴影 */
  margin: 10px;
}

textarea {
  flex: 1;
  font-size: 16px;
  background-color: #ececec;
  border: 0px ; 
  margin: 0 10px;
  resize: none;
  outline: none;
}

.send-button {
  padding: 6px;
  background-color: cornflowerblue;
  border-radius: 50%; /* 圆形按钮 */
  border: none;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.send-button img {
  width: 16px;
  height: 16px;
}

.send-button:hover {
  background-color: #285bb5; /* 鼠标悬停时颜色变深 */
}

</style>


<!-- components/Assistant.vue -->
<template>

    <div class="assistant-container">
      <img src="/assets/icons/chatgpt.svg" alt="Assistant Avatar" class="avatar" />
      <div class="assistant-message" v-html="result"></div>
    </div>
    <div class="assistant-footer">
      <button class="btn copy-all" :data-clipboard-text="content" @click="copyText()">
        <img :src="copyIcon"/>
      </button>
      <button class="btn switch-model">
        <img src="/assets/icons/switch.svg" />
      </button>
    </div>


</template>

<script setup>
import { computed, onMounted, onBeforeUnmount } from "vue";
import MarkdownIt from "markdown-it";
import hljs from 'highlight.js';
import 'highlight.js/styles/atom-one-dark.css';
import Clipboard from 'clipboard'
const props = defineProps({
  content: {
    type: String,
  }
});

var md = new MarkdownIt({
  html: false,
  linkify: true,
  typographer: true,
  breaks: true,
  highlight: function (str, lang) {
    if (lang && hljs.getLanguage(lang)) {
      try {
        return `<pre class="hljs" style="border-radius: 8px;">
        <div style="display:flex; justify-content: space-between; font-size: 11px;">
          <p style="padding-left: 10px;">${lang}</p>
          <p class="copy-btn" data-clipboard-action="copy" data-clipboard-text="${md.utils.escapeHtml(str)}" 
          style="padding-right: 10px; cursor: pointer;">${codeMessage.value}</p>
        </div>
        <code class="language-${lang} hljs" style="border-radius: 8px;">` +
          hljs.highlight(str, { language: lang, ignoreIllegals: true }).value +
          '</code></pre>';
      } catch (__) { }
    }
    return '<pre style="border-radius: 8px;  background-color: green;"><code class="language-none hljs" style="border-radius: 8px;">' + md.utils.escapeHtml(str) + '</code></pre>';
  }

});

let copyIcon = ref('/_nuxt/assets/icons/copy.svg')
let codeMessage = ref("复制")

function copyText() {
  const clipboard = new Clipboard('.copy-all');

  clipboard.on('success', () => {
    // alert('复制成功');
    copyIcon.value= "/_nuxt/assets/icons/copyend.svg";
    setTimeout(() => {
      copyIcon.value= "/_nuxt/assets/icons/copy.svg";
    }, 2000);
    clipboard.destroy();
  });

  clipboard.on('error', () => {
    alert('复制失败');
    clipboard.destroy();
  });

}

let clipboard = null;
onMounted(() => {
  clipboard = new Clipboard('.copy-btn'); // 直接绑定复制按钮

  clipboard.on('success', (e) => {
    codeMessage.value = "已复制"; // 更新按钮文本
    setTimeout(() => {
      codeMessage.value = "复制"; // 一定时间后恢复按钮文本
    }, 2000);
    clipboard.destroy()
  });

  clipboard.on('error', () => {
    alert('复制失败');
    clipboard.destroy() 
  });
});

const result = computed(() => md.render(props.content));


</script>

<style scoped>
/* 容器样式 */
.assistant-container {
  display: flex;
  align-items: flex-start;
  margin: 10px 0;
}
.avatar {
  width: 30px;
  height: 30px;
  padding: 5px;
  border: 1px solid #ccc;
  border-radius: 50%;
  margin-right: 10px;
}

.assistant-message {
  /* background-color: #e7e7e7; */
  padding: 10px;
  border-radius: 10px;
  max-width: 80%;
  word-wrap: break-word;
  line-height: 1.5;
  font-family: 'Arial', sans-serif;
}

.assistant-footer{
  display: flex;
  margin-left: 50px;
}

.btn {
  cursor: pointer;
  background-color: #fff;
  border: none;
  margin: 10px;
  padding-top: 5px;
}
.btn:hover {
  background-color: #f3f4f6;
  border-radius: 5px;
}



@media (max-width: 768px) {
  .assistant-container {
    flex-direction: column;
  }

  .assistant-message {
    font-display: block;
    overflow-wrap: break-all;
    color: red;
    max-width: 90%;
    white-space: normal;
  }

  .avatar {
    margin-bottom: 10px;
  }
  
  .assistant-footer{
    margin-left: 0;
  }
}
</style>
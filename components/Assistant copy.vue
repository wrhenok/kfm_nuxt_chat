<!-- components/Assistant.vue -->
<template>

  <div class="assistant-container">
    <img src="/assets/icons/chatgpt.svg" alt="Assistant Avatar" class="avatar" />
    <div class="assistant-message" v-html="result"></div>

  </div>


</template>

<script setup>
import { computed, onMounted, onBeforeUnmount } from "vue";
import MarkdownIt from "markdown-it";
import hljs from 'highlight.js';
import 'highlight.js/styles/atom-one-dark.css';
import Shiki from '@shikijs/markdown-it';
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
        return `<pre class="kfmhljs"><code class="language-${lang} hljs">` +
          hljs.highlight(str, { language: lang, ignoreIllegals: true }).value +
          '</code></pre>';
      } catch (__) { }
    }

    return '<pre><code class="language-none hljs">' + md.utils.escapeHtml(str) + '</code></pre>';
  }
});

const md = new MarkdownIt({
  html: true,
  linkify: true,
  typographer: true,
  highlight: function (str, lang) {
    // 当前时间加随机数生成唯一的id标识
    const codeIndex = parseInt(Date.now()) + Math.floor(Math.random() * 10000000)
    
    let html = `<button class="copy-btn" type="button" data-clipboard-target="#copy${codeIndex}">复制</button>`
    const linesLength = str.split(/\n/).length - 1
    // 生成行号
    let linesNum = '<span aria-hidden="true" class="line-numbers-rows">'
    for (let index = 0; index < linesLength; index++) {
      linesNum = linesNum + '<span></span>'
    }
    linesNum += '</span>'
    if (lang && hljs.getLanguage(lang)) {
      try {
        // highlight.js 高亮代码
        const preCode = hljs.highlight(lang, str, true).value
        html = html + preCode
        if (linesLength) {
          html += '<b class="name">' + lang + '</b>'
        }
        // 将代码包裹在 textarea 中
        return `<pre class="hljs"><code>${html}</code>${linesNum}</pre><textarea style="position: absolute;top: -9999px;left: -9999px;z-index: -9999;" id="copy${codeIndex}">${str.replace(/<\/textarea>/g, '&lt;/textarea>')}</textarea>`
      } catch (error) {
        console.log(error)
      }
    }

    const preCode = md.utils.escapeHtml(str)
    html = html + preCode
    // 将代码包裹在 textarea 中
    return `<pre class="hljs"><code>${html}</code>${linesNum}</pre><textarea style="position: absolute;top: -9999px;left: -9999px;z-index: -9999;" id="copy${codeIndex}">${str.replace(/<\/textarea>/g, '&lt;/textarea>')}</textarea>`
  }
})



let clipboard = null

onMounted(() => {
  clipboard = new Clipboard('.copy-btn')

  clipboard.on('success', () => {
    alert('复制成功')
    // $message.success('复制成功') // 使用 Nuxt 提供的全局消息提示
  })

  clipboard.on('error', () => {
    alert('复制失败')
    $message.error('复制失败')
  })
})

onBeforeUnmount(() => {
  if (clipboard) {
    clipboard.destroy() // 销毁 Clipboard 实例
  }
})

// md.use(await Shiki({
//   themes: {
//     light: 'vitesse-light',
//     dark: 'vitesse-dark',
//   }
// }))

const result = computed(() => {

  let html = md.render(props.content)

  return html;
});

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
  background-color: #e7e7e7;
  padding: 10px;
  border-radius: 10px;
  max-width: 80%;
  word-wrap: break-word;
  line-height: 1.5;
  font-family: 'Arial', sans-serif;
  /* 可以根据需要更改字体 */
}

@media (max-width: 768px) {
  .assistant-message {
    font-display: block;
    overflow-wrap: break-all;
    color: red;
    white-space: normal;

  }

  img .avatar {
    display: none;
  }
}

pre.hljs {
  padding: 12px 2px 12px 40px !important;
  border-radius: 5px !important;
  position: relative;
  font-size: 14px !important;
  line-height: 22px !important;
  overflow: hidden !important;
  code {
    display: block !important;
    margin: 0 10px !important;
    overflow-x: auto !important;
  }
  .line-numbers-rows {
    position: absolute;
    pointer-events: none;
    top: 12px;
    bottom: 12px;
    left: 0;
    font-size: 100%;
    width: 40px;
    text-align: center;
    letter-spacing: -1px;
    border-right: 1px solid rgba(0, 0, 0, .66);
    user-select: none;
    counter-reset: linenumber;
    span {
      pointer-events: none;
      display: block;
      counter-increment: linenumber;
      &:before {
        content: counter(linenumber);
        color: #999;
        display: block;
        text-align: center;
      }
    }
  }
  b.name {
    position: absolute;
    top: 2px;
    right: 50px;
    z-index: 10;
    color: #999;
    pointer-events: none;
  }
  .copy-btn {
    position: absolute;
    top: 2px;
    right: 4px;
    z-index: 10;
    color: #333;
    cursor: pointer;
    background-color: #fff;
    border: 0;
    border-radius: 2px;
  }
}

</style>
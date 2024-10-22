<!-- components/SelectModel.vue -->
<template>
  <select v-model="model" class="select-model">
    <option value="gqwen2">qwen2</option>
    <option value="llama3">llama3</option>
    <option value="gpt-4">GPT-4</option>
  </select>
</template>

<script setup>
import { computed } from 'vue';
import { useSelectModelStore } from '~/store/selectModel';

const selectModelStore = useSelectModelStore();

const model = computed({
  get: () => selectModelStore.selectedModel,
  set: (value) => selectModelStore.setModel(value),
});
</script>

<style scoped>
.select-model {
  appearance: none; /* 隐藏默认的下拉箭头 */
  border: 1px;
  margin: 5px;
  border-radius: 8px;
  padding: 10px 25px 10px 10px;
  font-size: 20px;
  color: grey;
  cursor: pointer;
  background-image: url(/assets/icons/arrow-down.svg);
  background-repeat: no-repeat;
  background-position: 95% center;
  background-size: 14px;
}

.select-model:hover {
  background-color: #eee;
}

.select-model:focus {
  outline: none;
  background-color: #eeeeee;
  box-shadow: 0px 0px 5px rgba(0, 123, 255, 0.5);
}

.select-model option {
  background-color: #ffffff;
  color: #333;
  padding: 10px;
}

</style>

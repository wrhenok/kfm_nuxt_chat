<!-- components/SelectRag.vue -->
<template>
  <select v-model="rag" class="select-model">
    <option value="rag-1">rag-1</option>
    <option value="rag-2">rag-2</option>
    <option value="rag-3">rag-3</option>
    <option value="rag-4">rag-4</option>
  </select>
</template>

<script setup>
import { useSelectRagStore } from '~/store/selectRag';

const selectRagStore = useSelectRagStore();
const rag = computed({
  get: () => selectRagStore.selectedRag,
  set: (value) => selectRagStore.setRag(value),
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
  /* transition: border-color 0.3s, box-shadow 0.3s;
  box-shadow: 0px 2px 8px rgba(0, 0, 0, 0.1); */
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
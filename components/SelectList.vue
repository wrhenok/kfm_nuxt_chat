<!-- components/SelectList.vue -->
<template>
  <select v-model="list" class="select-model">
    <option value="list1">列表 1</option>
    <option value="list2">列表 2</option>
    <option value="list3">列表 3</option>
    <option value="list4">列表 4</option>
    <option value="list5">列表 5</option>
  </select>
</template>

<script setup>
import { useSelectListStore } from '~/store/selectList';

const selectListStore = useSelectListStore();
const list = computed({
  get: () => selectListStore.list,
  set: (value) => selectListStore.setList(value),
});
</script>

<style scoped>
.select-model {
  appearance: none; /* 隐藏默认的下拉箭头 */
  text-align: center;
  border: 1px;
  border-radius: 8px;
  padding: 10px;
  font-size: 14px;
  color: grey;
  cursor: pointer;
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

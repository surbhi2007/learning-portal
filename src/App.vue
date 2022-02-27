<template>
  <div class="d-flex row p-3">
      <left-panel />
    <div class="d-flex mr-2">
      <right-panel />
    </div>
  </div>
</template>

<script setup>
import LeftPanel from './components/LeftPanel'
import RightPanel from './components/RightPanel'
import { provide, ref } from 'vue';

const selectedCategory = ref("All");
const searchText = ref("");

provide('selectedCategory', selectedCategory);
provide('searchText', searchText);

</script>

<style>
#app {
  background-color: #8080802e;
  height: 100vh;
  font-family: serif;
  line-height: 24px;
  width: 100%;
}
.d-flex {
  display: flex;
}
.row {
  flex-direction: row;
}
.mr-2 {
  margin-right: 16px;
}
</style>

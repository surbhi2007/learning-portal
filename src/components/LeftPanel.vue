<template>
  <div class="px-2">
    <span class="d-flex row"
      ><div>&#128269;</div>
      <input
        type="text"
        class="searchBar"
        v-model="searchText"
        placeholder="Filter Courses"
    /></span>
    <div v-for="(category, index) in categories.data" :key="index">
      <input
        :id="category + index"
        type="radio"
        :value="category"
        v-model="selectedCategory"
      />
      <label :for="category + index">{{ category }}</label>
    </div>
  </div>
</template>

<script setup>
import { onMounted, reactive, watch, inject } from "vue";
const categories = reactive({ data: [], error: null });

const selectedCategory = inject("selectedCategory");
const searchText = inject("searchText");

onMounted(async () => {
  // to fetch categories
  try {
    const response = await fetch(
      "https://frontend-hiring.appspot.com/all_categories?secret=HelloMars",
      {
        "Access-Control-Allow-Origin": "*",
      }
    );
    const resp = await response.json();
    categories.data = [...JSON.parse(resp.payload), "All"].sort();
  } catch (err) {
    categories.error = err;
  }
});
watch(searchText, (newValue) => {
  searchText.value = newValue.toLowerCase();
});
watch(selectedCategory, (newValue) => {
  selectedCategory.value = newValue;
});
</script>

<style scoped>
.px-2 {
  padding: 0 16px;
}
.searchBar {
  border: none;
  background-color: inherit;
  border-bottom: 2px solid black;
  margin-bottom: 16px;
  line-height: 20px;
}
input[type="text"]:focus {
  outline: none;
}
</style>

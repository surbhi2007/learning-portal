<template>
  <div class="p-2">
    <label for="search">Search for keywords</label>
    <span class="d-flex row py-2"
      ><div>&#128269;</div>
      <input
        type="text"
        id="search"
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
.py-2 {
  padding: 16px 0;
}
.p-2 {
  padding: 16px;
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

<template>
  <div class="d-flex row p-3">
    <div class="px-2">
      <input type="text" />
      <div v-for="(category, index) in categories.data" :key="index">
        <input
          :id="category + index"
          type="radio"
          :value="category"
          class="searchBox"
          v-model="selectedCategory"
        />
        <label :for="category + index">{{ category }}</label>
      </div>
    </div>
    <div class="d-flex">
    <div v-for="(course, index) in allCourses.data" :key="index">
      <div class="course-card mx-2">
        <div>{{ course.title }}</div>
        <div>{{ course.instructor_name }}</div>
        <p v-html="course.description"></p>
        <div>{{ course.start_date }}</div>
        <span>{{
          calculateWeeks(course.start_date, course.end_date) +
          "," +
          course.estimated_workload
        }}</span>
      </div>
    </div>
    </div>
  </div>
</template>

<script setup>
import { onMounted, reactive, ref } from "vue";
const categories = reactive({ data: [], error: null });
const allCourses = reactive({ data: [], error: null });
const selectedCategory = ref("All");

onMounted(async () => {
  // to fetch categories
  try {
    const response = await fetch(
      "https://frontend-hiring.appspot.com/all_categories?secret=HelloMars"
    );
    const resp = await response.json();
    categories.data = [...JSON.parse(resp.payload), "All"].sort();
  } catch (err) {
    categories.error = err;
  }
  // to fetch all courses
  try {
    const response = await fetch(
      "https://frontend-hiring.appspot.com/all_courses?secret=HelloMars"
    );
    const resp = await response.json();
    allCourses.data = JSON.parse(resp.payload);
  } catch (err) {
    allCourses.error = err;
  }
});
const calculateWeeks = (startDate, endDate) => {
  const noOfWeeks = Math.round(
    (new Date(endDate) - new Date(startDate)) / (7 * 24 * 60 * 60 * 1000)
  );
  return noOfWeeks + noOfWeeks > 1 ? " Weeks" : " Week";
};
</script>

<style>
#app {
  background-color: #8080802e;
  height: 100vh;
  width: auto;
}
.d-flex {
  display: flex;
}
.row {
  flex-direction: row;
}
.overflow-wrap {
  overflow: wrap;
  overflow: visible;
}
.course-card {
  border: 1px solid white;
  padding: 12px;
  background-color: #fff;
  box-shadow: 3px 0px 0px 2px #fff;
  width: 220px;
  height: 250px;
}
.px-2 {
  padding: 0 16px;
}
.p-3 {
  padding: 24px;
}
.mx-2 {
  margin: 0 16px;
}
</style>

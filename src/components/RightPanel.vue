<template>
  <div>
    <h2 class="italic">
      {{ filteredSearchedCategories.length + " courses open for registration" }}
    </h2>
    <div
      class="course-card m-3"
      v-for="(course, index) in filteredSearchedCategories"
      :key="index"
    >
      <div class="f-16">{{ course.title }}</div>
      <div>{{ course.instructor_name }}</div>
      <p v-html="course.description"></p>
      <div class="bold">Pre-registration</div>
      <div class="bold">
        {{
          getMonthYear(course.start_date) +
          " - " +
          getMonthYear(course.end_date)
        }}
      </div>
      <span class="italic">{{
        calculateWeeks(course.start_date, course.end_date) +
        ", " +
        course.estimated_workload
      }}</span>
    </div>
  </div>
</template>

<script setup>
import { onMounted, reactive, inject, computed } from "vue";
const allCourses = reactive({ data: [], error: null });
const selectedCategory = inject("selectedCategory");
const searchText = inject("searchText");

onMounted(async () => {
  // to fetch all courses
  try {
    const response = await fetch(
      "https://frontend-hiring.appspot.com/all_courses?secret=HelloMars",
      {
        "Access-Control-Allow-Origin": "*",
      }
    );
    const resp = await response.json();
    allCourses.data = JSON.parse(resp.payload);
  } catch (err) {
    allCourses.error = err;
  }
});
const filteredCategories = computed(() => {
  return selectedCategory.value !== "All"
    ? allCourses.data.filter((item) => item.category === selectedCategory.value)
    : allCourses.data;
});
const filteredSearchedCategories = computed(() => {
  return searchText.value && filteredCategories.value.length
    ? filteredCategories.value.filter(
        (item) =>
          item.title.toLowerCase().includes(searchText.value) ||
          item.instructor_name.toLowerCase().includes(searchText.value)
      )
    : filteredCategories.value;
});
const calculateWeeks = (startDate, endDate) => {
  const noOfWeeks = Math.round(
    (new Date(endDate) - new Date(startDate)) / (7 * 24 * 60 * 60 * 1000)
  );
  return noOfWeeks + (noOfWeeks > 1 ? " Weeks" : " Week");
};
const getMonthYear = (date) => {
  const dateArray = date.split("-");
  switch (dateArray[1]) {
    case "01":
      return "January " + dateArray[2];
    case "02":
      return "February " + dateArray[2];
    case "03":
      return "March " + dateArray[2];
    case "04":
      return "April " + dateArray[2];
    case "05":
      return "May " + dateArray[2];
    case "06":
      return "June " + dateArray[2];
    case "07":
      return "July " + dateArray[2];
    case "08":
      return "August " + dateArray[2];
    case "09":
      return "September " + dateArray[2];
    case "10":
      return "October " + dateArray[2];
    case "11":
      return "November " + dateArray[2];
    case "12":
      return "December " + dateArray[2];
  }
  return dateArray[1] + " " + dateArray[2];
};
</script>

<style scoped>
.course-card {
  border: 1px solid white;
  padding: 12px;
  background-color: #fff;
  box-shadow: -3px -5px 6px 6px #fff;
  width: 250px;
  height: auto;
  display: inline-block;
}
.course-card:hover {
  box-shadow: -3px -5px 6px 6px teal;
}
.m-3 {
  margin: 24px 24px;
}
.bold {
  font-weight: 900;
}
.f-16 {
  font-size: 24px;
}
.italic {
  font-style: italic;
}
h2 {
  font-weight: 500;
}
</style>

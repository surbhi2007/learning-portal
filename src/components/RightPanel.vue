<template>
  <div v-for="(course, index) in allCourses.data" :key="index">
    <div class="course-card mx-2">
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
      <span>{{
        calculateWeeks(course.start_date, course.end_date) +
        ", " +
        course.estimated_workload
      }}</span>
    </div>
  </div>
</template>

<script setup>
import { onMounted, reactive } from "vue";
const allCourses = reactive({ data: [], error: null });

onMounted(async () => {
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
  box-shadow: 3px 0px 0px 2px #fff;
}
.p-3 {
  padding: 24px;
}
.mx-2 {
  margin: 0 16px;
}
.bold {
  font-weight: 800;
}
.f-16 {
  font-size: 24px;
}
</style>

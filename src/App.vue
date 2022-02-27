<template>
  <div class="d-flex row p-3">
      <left-panel />
    <div class="d-flex">
      <right-panel />
    </div>
  </div>
</template>

<script>
import LeftPanel from './components/LeftPanel'
import RightPanel from './components/RightPanel'

 export default {
   components: {
     RightPanel,
     LeftPanel,
   }
 }

</script>

<style>
#app {
  background-color: #8080802e;
  height: 100vh;
  width: auto;
  font-family: cursive;
}
.d-flex {
  display: flex;
}
.row {
  flex-direction: row;
}
.course-card {
  border: 1px solid white;
  padding: 12px;
  background-color: #fff;
  box-shadow: 3px 0px 0px 2px #fff;
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
.bold {
  font-weight: 800;
}
.searchBar {
  border: none;
  background-color: inherit;
  border-bottom: 2px solid black;
  margin-bottom: 16px;
  line-height: 20px;
}
input[type=text]:focus{
  outline: none;
}
.f-16 {
  font-size: 24px;
}
</style>

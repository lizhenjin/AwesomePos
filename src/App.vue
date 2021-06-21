<template>
  <div id="app">
    <el-container style="height: 100%;">
      <el-aside width="170px" style="height: 100%;background-color:#545c64"><leftNav></leftNav></el-aside>
      <el-main><router-view /></el-main>
    </el-container>
  </div>
</template>

<script>
import leftNav from "@/components/common/leftNav";
export default {
  name: "App",
  components: {
    leftNav,
  },
};
</script>


<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
.el-main{
  padding:0;
}
</style>

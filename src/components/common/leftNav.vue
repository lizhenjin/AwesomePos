<template>
    <el-menu
      default-active="1"
      class="el-menu-vertical-demo"
      @open="handleOpen"
      @close="handleClose"
      background-color="#545c64"
      text-color="#fff"
      active-text-color="#ffd04b">
      <el-menu-item index="1">
        <i class="icon iconfont icon-shouyin"></i>
        <span slot="title">收银</span>
      </el-menu-item>
      <el-menu-item index="2">
         <i class="icon iconfont icon-dianpu"></i>
        <span slot="title">店铺</span>
      </el-menu-item>
      <el-menu-item index="3">
        <i class="icon iconfont icon-shangpin"></i>
        <span slot="title">商品</span>
      </el-menu-item>
      <el-menu-item index="4">
        <i class="icon iconfont icon-membership-card_icon"></i>
        <span slot="title">会员</span>
      </el-menu-item>
       <el-menu-item index="5">
        <i class="icon iconfont icon-tongji"></i>
        <span slot="title">统计</span>
      </el-menu-item>
       <el-menu-item index="6">
        <i class="icon iconfont icon-shezhi"></i>
        <span slot="title">设置</span>
      </el-menu-item>
    </el-menu>
</template>
<script>
  export default {
    methods: {
      handleOpen(key, keyPath) {
        console.log(key, keyPath);
      },
      handleClose(key, keyPath) {
        console.log(key, keyPath);
      }
    }
  }
</script>
<style scoped>
 .el-menu{
   border-right: solid 1px #545c64
 }

</style>
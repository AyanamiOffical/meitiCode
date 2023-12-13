<template>
  <el-menu
      :router="true"
      class="el-menu-vertical-demo"
      :default-active="route.path"
      text-color=""
  >

    <el-sub-menu index = "1" >
      <template #title>
        <el-icon><Refrigerator /></el-icon>
        <span >仓库 数据 </span>
      </template>
      <el-menu-item index="/side/ware/hardware">
        <el-icon><Connection /></el-icon>入库记录 查看</el-menu-item>
      <el-menu-item index="/side/ware/manifest">
        <el-icon><HomeFilled /></el-icon>仓库 货品</el-menu-item>
    </el-sub-menu>
    <el-menu-item index="/side/home" >
      <el-icon><Cpu /></el-icon>
      <div>员工 数据</div>
    </el-menu-item>
    <el-menu-item index="/side/calendar">
      <el-icon><document /></el-icon>
      <div>轮班表 查看</div>
    </el-menu-item>
    <el-menu-item index="/side/action">
      <el-icon><paperclip /></el-icon>
      <div>员工请求 处理</div>
    </el-menu-item>
    <el-menu-item index="/side/result">
      <el-icon><ChatLineRound /></el-icon>
      <div>员工请求 查询</div>
    </el-menu-item>
    <el-menu-item index="/side/self">
      <el-icon><User /></el-icon>
      <div>个人 信息</div>
    </el-menu-item>
  </el-menu>
</template>

<script setup>
import { useRoute,useRouter } from 'vue-router';
import {
  Back, ChatLineRound,
  Connection,
  Cpu,
  Document,
  HomeFilled,
  Location,
  Paperclip, Refrigerator,
  ShoppingTrolley,
  User
} from "@element-plus/icons-vue";
import Homepage from "./workers/homepage.vue";

const route = useRoute();

</script>
<style scoped>
.el-menu-vertical-demo{
  box-shadow: 5px 0 5px -5px #000;
}
</style>
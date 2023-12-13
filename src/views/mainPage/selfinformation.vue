<template>
  <el-descriptions title="个人信息显示" :column="3" border>
    <el-descriptions-item
        label="用户"
        label-align="right"
        align="center"
        label-class-name="my-label"
        class-name="my-content"
        width="150px"
    >浮世英寿</el-descriptions-item
    >
    <el-descriptions-item label="联系方式" label-align="right" align="center"
    >18100000000</el-descriptions-item
    >
    <el-descriptions-item label="工作地点" label-align="right" align="center"
    >ZhengZhou</el-descriptions-item
    >
    <el-descriptions-item label="部门标签" label-align="right" align="center">
      <el-tag size="small">Office</el-tag>
    </el-descriptions-item>
    <el-descriptions-item label="地址" label-align="right" align="center"
    >No.0002, DongFeng Avenue, JinShui District, ZhengZhou, HeNan
      Province</el-descriptions-item
    >
  </el-descriptions>
</template>
<style scoped>

:deep(.my-label) {
  background: var(--el-color-success-light-9) !important;
}
:deep(.my-content) {
  background: var(--el-color-danger-light-9);
}
</style>
<script setup>
</script>
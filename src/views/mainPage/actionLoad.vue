<template>
  <el-form :model="form" label-width="120px">
    <el-form-item label="活动 名称">
      <el-input v-model="form.activityName" />
    </el-form-item>
    <el-form-item label="活动 形式" label-width="120px">
      <el-input v-model="form.activityType" />
    </el-form-item>
    <el-form-item label="业务 位置">
      <el-select v-model="form.activityZone" placeholder="请选择活动地点">
        <el-option label="仓库1" value="仓库one" />
        <el-option label="仓库2" value="仓库two" />
        <el-option label="仓库3" value="仓库three" />
      </el-select>
    </el-form-item>
    <el-form-item label="活动 时间">
      <el-col :span="11">
        <el-date-picker
            v-model="form.activityTime"
            type="date"
            placeholder="选择日期"
            style="width: 100%"
        />
      </el-col>
      <el-col :span="2" class="text-center">
        <span class="text-gray-500">-</span>
      </el-col>
      <el-col :span="11">
        <el-date-picker
            v-model="form.activityTime"
            placeholder="选择时间"
            style="width: 100%"
            value-format="YYYY-MM-DD HH:mm:ss"
            type="datetime"
        />
      </el-col>
    </el-form-item>

    <el-form-item label="资源 申请">
      <el-radio-group v-model="form.resource">
        <el-radio label="赞助商" />
        <el-radio label="地址" />
      </el-radio-group>
    </el-form-item>
    <el-form-item label="活动 形式">
      <el-input v-model="form.activityForm" type="textarea" />
    </el-form-item>
    <el-form-item>
      <el-button type="primary" @click="onSubmit">提交</el-button>
      <el-button>取消</el-button>
    </el-form-item>
  </el-form>
</template>

<script setup>
import { ref } from 'vue';
import axios from 'axios';

const form = ref({
  activityName: '',
  activityZone: '',
  activityTime: '',
  activityType: '',
  activityForm: '',
});

const onSubmit = async () => {
  try {
    // 使用Axios向后端API端点发送POST请求
    const response = await axios.post('http://localhost:8081/activity/add', form.value);

    // 根据需要处理响应（例如，显示成功消息，导航到另一页）
    console.log('提交成功：', response.data);
  } catch (error) {
    console.error('提交表单时出错：', error);
    // 处理错误（例如，向用户显示错误消息）
  }
};
</script>
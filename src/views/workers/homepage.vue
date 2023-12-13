<template>
  <div>
    <el-button @click="showAddEmployeeDialog">添加员工</el-button>

    <el-table :ref="tableRef" row-key="id" :data="tableData" style="width: 100%">
      <el-table-column label="序号" width="80">
        <template #default="scope">
          {{ scope.$index + 1 }}
        </template>
      </el-table-column>
      <el-table-column
          prop="employeeName"
          label="人员名称"
          sortable
          width="180"
      />
      <el-table-column
          prop="age"
          label="年龄"
          sortable
          width="80"
      />
      <el-table-column
          prop="gender"
          label="性别"
          sortable
          width="80"
      />
      <el-table-column
          prop="address"
          label="地址"
          :formatter="formatter"
      />
      <el-table-column
          prop="manHour"
          label="工时"
          sortable
          width="120"
      />
    </el-table>

    <el-dialog
        title="添加员工"
        :visible.sync="dialogVisible"
        width="30%"
        :before-close="handleCloseDialog"
    >
      <el-form :model="newEmployeeForm" ref="newEmployeeFormRef" :rules="newEmployeeFormRules">
        <el-form-item label="人员名称" prop="employeeName">
          <el-input v-model="newEmployeeForm.employeeName"></el-input>
        </el-form-item>
        <el-form-item label="年龄" prop="age">
          <el-input v-model="newEmployeeForm.age"></el-input>
        </el-form-item>
        <el-form-item label="性别" prop="gender">
          <el-radio-group v-model="newEmployeeForm.gender">
            <el-radio label="male">男</el-radio>
            <el-radio label="female">女</el-radio>
          </el-radio-group>
        </el-form-item>
        <el-form-item label="地址" prop="address">
          <el-input v-model="newEmployeeForm.address"></el-input>
        </el-form-item>
        <el-form-item label="工时" prop="manHour">
          <el-input v-model="newEmployeeForm.manHour"></el-input>
        </el-form-item>
      </el-form>

      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible = false">取消</el-button>
        <el-button type="primary" @click="addEmployee">添加</el-button>
      </div>
    </el-dialog>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue'
import axios from 'axios'
import type { TableColumnCtx, TableInstance } from 'element-plus'
const tableRef = ref<TableInstance>()
const tableData = ref([])
const dialogVisible = ref(false)
const newEmployeeForm = ref({
  employeeName: '',
  age: '',
  gender: 'male',
  address: '',
  manHour: ''
})

const newEmployeeFormRules = ref({
  employeeName: [{ required: true, message: '请输入人员名称', trigger: 'blur' }],
  age: [{ required: true, message: '请输入年龄', trigger: 'blur' }],
  gender: [{ required: true, message: '请选择性别', trigger: 'change' }],
  address: [{ required: true, message: '请输入地址', trigger: 'blur' }],
  manHour: [{ required: true, message: '请输入工时', trigger: 'blur' }]
})

const loadData = async () => {
  try {
    const response = await axios.get('http://localhost:8081/employee/list') // Replace with your actual endpoint
    tableData.value = response.data
  } catch (error) {
    console.error('Error fetching data from the server:', error)
  }
}

const formatter = (row, column: TableColumnCtx<any>) => {
  return row.address
}

const showAddEmployeeDialog = () => {
  dialogVisible.value = true;
}

const handleCloseDialog = () => {
  newEmployeeFormRef.value?.clearValidate(); // Clear form validation
  newEmployeeForm.value = {
    employeeName: '',
    age: '',
    gender: 'male',
    address: '',
    manHour: ''
  }
}

const addEmployee = () => {
  newEmployeeFormRef.value?.validate(async (valid) => {
    if (valid) {
      // Perform the logic to add the new employee
      // You can send a request to the server here
      // After adding, close the dialog and reload the data if necessary
      // For demonstration purposes, just closing the dialog
      dialogVisible.value = false
      await loadData(); // Reload data after adding employee
    }
  });
}

onMounted(() => {
  loadData()
})
</script>

<style scoped>
</style>

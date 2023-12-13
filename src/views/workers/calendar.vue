<template>
  <div>
    <el-table :data="mergedData" style="width: 100%" border>
      <el-table-column prop="day" label="Day"></el-table-column>
      <el-table-column label="Morning">
        <template #default="scope">
          <div class="time-cell" @click="openPopup(scope.row, 'morning')">
            {{ scope.row.morningPerson }}
          </div>
        </template>
      </el-table-column>
      <el-table-column label="Afternoon">
        <template #default="scope">
          <div class="time-cell" @click="openPopup(scope.row, 'afternoon')">
            {{ scope.row.afternoonPerson }}
          </div>
        </template>
      </el-table-column>
      <el-table-column label="Evening">
        <template #default="scope">
          <div class="time-cell" @click="openPopup(scope.row, 'evening')">
            {{ scope.row.eveningPerson }}
          </div>
        </template>
      </el-table-column>
    </el-table>

    <el-dialog v-model="popupVisible" title="输入员工">
      <el-form ref="popupForm" :model="popupForm" label-width="80px">
        <el-form-item label="Name" prop="employeeName">
          <el-input v-model="popupForm.employeeName"></el-input>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="popupVisible = false">取消</el-button>
        <el-button type="primary" @click="saveData">保存</el-button>
      </div>
    </el-dialog>
    <el-button type="primary" @click="randomFill">随机填充员工</el-button>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      leftData: [
        { day: 'Monday' },
        { day: 'Tuesday' },
        { day: 'Wednesday' },
        { day: 'Thursday' },
        { day: 'Friday' },
      ],
      tableData: [
        { morningPerson: '', afternoonPerson: '', eveningPerson: '', time: '' },
        { morningPerson: '', afternoonPerson: '', eveningPerson: '', time: '' },
        { morningPerson: '', afternoonPerson: '', eveningPerson: '', time: '' },
        { morningPerson: '', afternoonPerson: '', eveningPerson: '', time: '' },
        { morningPerson: '', afternoonPerson: '', eveningPerson: '', time: '' },
      ],
      popupVisible: false,
      popupForm: {
        employeeName: '',
        time: '',
      },
      selectedRow: null,
      selectedPeriod: null,
    };
  },

  computed: {
    mergedData() {
      // Combine leftData with corresponding data from tableData
      return this.leftData.map((day, index) => ({
        ...day,
        ...this.tableData[index],
      }));
    },
  },

  methods: {
    async fetchEmployeeData() {
      try {
        const response = await axios.get('http://localhost:8081/employee/list');
        console.log(response);
        return response.data;
      } catch (error) {
        console.error('Error fetching employee data:', error);
        return [];
      }
    },

    async randomFill() {
      const employeeData = await this.fetchEmployeeData();

      if (employeeData.length > 0) {
        this.tableData.forEach((row) => {
          ['morning', 'afternoon', 'evening'].forEach((period) => {
            const randomIndex = Math.floor(Math.random() * employeeData.length);
            const randomEmployee = employeeData[randomIndex];
            row[`${period}Person`] = randomEmployee.employeeName;
          });
        });
      }

      console.log(this.tableData); // Log the updated tableData to the console
    },

    openPopup(row, period) {
      this.selectedRow = row;
      this.selectedPeriod = period;
      this.popupForm.employeeName = row[`${period}Person`];
      this.popupForm.time = row[period];
      this.popupVisible = true;
    },

    saveData() {
      if (this.selectedRow && this.selectedPeriod !== null) {
        this.selectedRow[`${this.selectedPeriod}Person`] = this.popupForm.employeeName;
        this.selectedRow[this.selectedPeriod] = this.popupForm.time;
      }

      this.popupVisible = false;
      this.resetPopupForm();
    },

    resetPopupForm() {
      this.popupForm = {
        employeeName: '',
        time: '',
      };
      this.selectedRow = null;
      this.selectedPeriod = null;
    },
  },
};
</script>

<style scoped>
.time-cell {
  border: 1px solid #e0e0e0;
  padding: 5px;
  text-align: center;
  cursor: pointer;
}

.el-table th,
.el-table td {
  border: 1px solid #e0e0e0;
}

.el-table th {
  background-color: #f5f5f5;
}
</style>

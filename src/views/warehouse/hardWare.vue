<template>
  <div>
    <el-table :data="tableData" style="width: 100%" max-height="250" max-width="200">
      <el-table-column fixed prop="id" label="货物号" width="150" show-overflow-tooltip/>
      <el-table-column prop="waybillName" label="货物名称" width="120" show-overflow-tooltip/>
      <el-table-column prop="waybillTime" label="收货时间" width="120" show-overflow-tooltip/>
      <el-table-column prop="consigneeName" label="收货人" width="400" show-overflow-tooltip/>
      <el-table-column prop="waybillDescription" label="货单描述" width="120" show-overflow-tooltip/>
      <el-table-column fixed="right" label="操作" width="120">
        <template #default="scope">
          <el-button
              link
              type="primary"
              size="small"
              @click.prevent="deleteRow(scope.row)"
          >
            移除
          </el-button>
        </template>
      </el-table-column>
    </el-table>
    <el-dialog v-model="deleteDialogVisible" title="确认删除">
      <p>确定要删除该项吗？</p>
      <div slot="footer" class="dialog-footer">
        <el-button @click="deleteDialogVisible = false">取消</el-button>
        <el-button type="primary" @click="confirmDelete">确定</el-button>
      </div>
    </el-dialog>
    <el-button class="mt-4" style="width: 100%" @click="showDialog">
      添加货物
    </el-button>

    <el-dialog v-model="dialogVisible" title="添加货物">
      <el-form ref="addForm" :model="addItemForm" label-width="80px">
        <el-form-item label="货物号" prop="id">
          <el-input v-model="addItemForm.id" style="width: 200px;" :placeholder="getNextIdPlaceholder()"></el-input>
        </el-form-item>
        <el-form-item label="货物名称" prop="waybillName">
          <el-input v-model="addItemForm.waybillName" style="width: 200px;"></el-input>
        </el-form-item>
        <el-form-item label="收货时间" prop="receiveTime">
          <el-date-picker v-model="addItemForm.receiveTime" type="datetime" placeholder="选择日期" value-format="YYYY-MM-DD HH:mm:ss"></el-date-picker>
        </el-form-item>
        <el-form-item label="收货人" prop="consigneeName">
          <el-input v-model="addItemForm.consigneeName" style="width: 300px;"></el-input>
        </el-form-item>
        <el-form-item label="货单描述" prop="waybillDescription">
          <el-input v-model="addItemForm.waybillDescription" style="width: 150px;"></el-input>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible = false">取消</el-button>
        <el-button type="primary" @click="addItem">添加</el-button>
      </div>
    </el-dialog>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      tableData: [],
      dialogVisible: false,
      addItemForm: {
        id: '',
        waybillName: '',
        receiveTime: '',
        consigneeName: '',
        waybillDescription: '',
      },
      deleteDialogVisible: false,
      deleteItemIndex: null,
      lastItemId: 0,
    };
  },
  mounted() {
    this.loadData();
  },
  methods: {
    async loadData() {
      try {
        const response = await axios.get('http://localhost:8081/waybill/list');
        this.tableData = response.data;
        this.lastItemId = Math.max(...this.tableData.map(item => item.id), 0);
      } catch (error) {
        console.error('Error fetching data from the server:', error);
      }
    },
    showDialog() {
      this.dialogVisible = true;
    },
    addItem() {
      if (!this.addItemForm.id) {
        this.addItemForm.id = this.lastItemId + 1;
      }
      this.tableData.push({ ...this.addItemForm });

      axios.post('http://localhost:8081/waybill/add', this.addItemForm)
          .then(() => {
            console.log('Successfully added item to the backend.');
          })
          .catch(error => {
            console.error('Error adding item to the backend:', error);
          })
          .finally(() => {
            this.dialogVisible = false;
            this.resetAddItemForm();
          });
    },
    resetAddItemForm() {
      this.addItemForm = {
        id: '',
        waybillName: '',
        receiveTime: '',
        consigneeName: '',
        waybillDescription: '',
      };
    },
    deleteRow(row) {
      this.deleteItemIndex = this.tableData.indexOf(row);
      this.deleteDialogVisible = true;
    },

    confirmDelete() {
      if (this.deleteItemIndex !== null) {
        const waybillIdToDelete = this.tableData[this.deleteItemIndex].id;

        axios.delete(`http://localhost:8081/waybill/${waybillIdToDelete}`)
            .then(() => {
              console.log('Successfully deleted item from the backend.');
            })
            .catch(error => {
              console.error('Error deleting item from the backend:', error);
            })
            .finally(() => {
              this.tableData.splice(this.deleteItemIndex, 1);
              this.deleteDialogVisible = false;
              this.deleteItemIndex = null;
            });
      }
    },
    getNextIdPlaceholder() {
      return `下一个货物号: ${this.lastItemId + 1}`;
    },
  },
};
</script>

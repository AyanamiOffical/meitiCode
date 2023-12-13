<template>
  <div>
    <el-card>
      <div slot="header">
        <h2>仓库货单</h2>
      </div>
      <el-button type="primary" @click="showDialog">添加</el-button>
      <el-button type="danger" @click="showDeleteDialog">删除</el-button>
      <el-table :data="tableData" border style="width: 100%">
        <!-- Existing table columns -->
        <el-table-column prop="id" label="货单编号" width="120"></el-table-column>
        <el-table-column prop="productName" label="产品名称"></el-table-column>
        <el-table-column prop="quantity" label="数量"></el-table-column>
        <el-table-column prop="productNumber" label="商品号"></el-table-column>
        <el-table-column prop="productAddress" label="商品地址"></el-table-column>
        <el-table-column label="操作" width="100">
          <template slot-scope="scope">
            <el-button type="text" @click="editItem(scope.row)">编辑</el-button>
            <el-button type="text" @click="removeItem(scope.$index)">删除</el-button>
          </template>
        </el-table-column>
      </el-table>

      <!-- Dialog for data input -->
      <el-dialog v-model="dialogVisible" title="添加货单">
        <el-form ref="formData" :model="form" label-width="80px">
          <!-- Existing form items -->
        </el-form>
        <div slot="footer" class="dialog-footer">
          <el-button @click="dialogVisible = false">取消</el-button>
          <el-button type="primary" @click="saveItem">保存</el-button>
        </div>
      </el-dialog>

      <!-- Dialog for deletion -->
      <el-dialog v-model="deleteDialogVisible" title="确认删除">
        <span>是否删除？</span>
        <div slot="footer" class="dialog-footer">
          <el-button @click="deleteDialogVisible = false">取消</el-button>
          <el-button type="danger" @click="confirmRemoveItem">是</el-button>
        </div>
      </el-dialog>
    </el-card>
  </div>
</template>

<script>
export default {
  data() {
    return {
      // Existing data
      deleteDialogVisible: false,
      deleteProductNumber: null,
    };
  },
  methods: {
    // Existing methods

    // Show dialog for deletion
    showDeleteDialog() {
      this.deleteDialogVisible = true;
    },

    // Confirm removal
    confirmRemoveItem() {
      // Remove item based on product number
      const index = this.tableData.findIndex(item => item.productNumber === this.deleteProductNumber);
      if (index !== -1) {
        this.tableData.splice(index, 1);
      }

      // Reset values
      this.deleteProductNumber = null;
      this.deleteDialogVisible = false;
    },
  },
};
</script>

<style>
/* Add styles if needed */
</style>

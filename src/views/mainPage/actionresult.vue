<!-- ActivityList.vue -->

<template>
  <div>
    <el-table :data="activities" style="width: 100%">
      <el-table-column prop="activityName" label="Activity Name"></el-table-column>
      <el-table-column prop="activityTime" label="Activity Time"></el-table-column>
      <el-table-column prop="activityType" label="Activity Type"></el-table-column>
      <el-table-column prop="activityForm" label="Activity Form"></el-table-column>
      <el-table-column prop="activityZone" label="Activity Zone"></el-table-column>
      <el-table-column label="Status" width="150">
        <template slot-scope="{ row }">
          <el-tag :type="row && row.status === 'processed' ? 'danger' : 'primary'">{{ row && row.status }}</el-tag>
          <el-button type="text" @click="editStatus(row)">编辑</el-button>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      activities: [], // Data will be fetched from the backend
    };
  },
  mounted() {
    // Fetch data from the backend when the component is mounted
    this.fetchActivities();
  },
  methods: {
    async fetchActivities() {
      try {
        // Make an Axios GET request to your backend API endpoint
        const response = await axios.get('http://localhost:8081/activity/list'); // Adjust the endpoint based on your actual API
        this.activities = response.data;
      } catch (error) {
        console.error('Error fetching activities:', error);
        // Handle error as needed
      }
    },
    editStatus(row) {
      // Add your logic to open an editing interface (e.g., a dialog) for the selected row
      console.log('Edit status for row:', row);
    },
  },
};
</script>

<style>
/* Add custom styles if needed */
</style>

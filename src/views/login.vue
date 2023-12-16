<template>
  <el-row class="login-container">
    <el-col :lg="16" :md="12" class="left">
      <div>
        <div class="bigTitle">Media Mall Management System</div>
        <div class="smallTitle">
          Dedicated to serving high-quality e-commerce backends
        </div>
      </div>
    </el-col>
    <el-col :lg="8" :md="12" class="right">
      <h1 class="title01">Welcome Back</h1>
      <span><el-avatar :size="60" :src="testImage" class="lay"/></span>
      <div class="title02">
        <span class="h-[1px] w-16 bg-gray-200 "></span>
        <span>Account Password Login</span>
        <span class="h-[1px] w-16 bg-gray-200"></span>
      </div>
      <div>
        <ul class="bg-squares">
          <li></li>
          <li></li>
          <li></li>
          <li></li>
          <li></li>
          <li></li>
          <li></li>
          <li></li>
          <li></li>
          <li></li>
        </ul>
      </div>
      <el-form ref="formRef" :rules="rules" :model="form" class="w-[250px]">
        <el-form-item prop="username">
          <el-input
              :prefix-icon="User"
              v-model="form.username"
              placeholder="Enter your username"
          />
        </el-form-item>
        <el-form-item prop="password">
          <el-input
              :prefix-icon="Lock"
              v-model="form.password"
              placeholder="Enter your password"
              show-password
              type="password"
          />
        </el-form-item>
        <el-form-item>
          <el-button
              round
              color="#626aef"
              class="w-[250px]"
              type="primary"
              @click="onSubmit"
          >Log In</el-button
          >
        </el-form-item>
      </el-form>
    </el-col>
  </el-row>
</template>

<script setup>
import { reactive, ref } from "vue";
import { Lock, User } from "@element-plus/icons-vue";
import router from "../router";
import axios from "axios"; // Import Axios for making HTTP requests
import testImage from "../assets/testImage.jpg"
import {useStore} from "vuex";

const form = reactive({
  username: "",
  password: "",
});

const rules = reactive({
  username: [
    {
      required: true,
      message: "Username cannot be empty",
      trigger: "blur",
    },
    {
      min: 2,
      max: 50,
      message: "Username length must be between 3-50 characters",
      trigger: "blur",
    },
  ],
  password: [
    {
      required: true,
      message: "Password cannot be empty",
      trigger: "blur",
    },
    {
      min: 3,
      max: 50,
      message: "Password length must be between 3-50 characters",
      trigger: "blur",
    },
  ],
});

const formRef = ref(null);
const store = useStore();

const onSubmit = async () => {
  formRef.value.validate(async (valid) => {
    if (!valid) {
      console.log('Form validation failed. Not redirecting.');
      return false
    }
    try {
      const response = await axios.post("http://localhost:8081/user/search", {
        username: form.username,
        password: form.password,
      });

      let found = false;
      let userId = null;

      for (let i = 0; i < response.data.length; i++) {
        if (response.data[i].username === form.username && response.data[i].password === form.password ) {
          found = true;
          userId = i;
          break; // Found a matching username, break the loop
        }
      }

      if (found) {
        // Login successful, store user information in Vuex
        store.commit('setUser', {
          username: form.username,
          usertype: response.data[userId].usertype, // Assuming the data returned has a userType field
        });

        ElMessage.success("Logged in successfully!");
        setTimeout(() => {
          router.push({ name: "allin" });
        }, 500);
      } else {
        ElMessage.error("Login failed. Invalid username or password");
      }
    } catch (error) {
      console.error("Error during authentication:", error);
      // Handle error as needed
    }
  });
};
</script>

<style lang="scss">
.lay{
  margin-top: 10px;
}
.login-container {
  height: 100%;
  width: 100%;
  border-radius: 50px;
  display: flex;
  background-image: linear-gradient(120deg, #e0c3fc 0%, #8ec5fc 64%);
  box-shadow: 0 0 30px rgba(0, 0, 0, 0.6); /* Add this line for the shadow */
  min-height: 100vh;
  @apply min-h-screen bg-blue-500 ;
  .left {
    @apply flex items-center   justify-center text-center;
    .bigTitle{
      @apply  justify-center font-bold text-5xl text-light-100 mb-4 ;
    }
    .smallTitle{
      @apply text-light-5000 font-bold
    }
  }
  .right {

    border-radius: 50px;
    @apply bg-light-500 flex items-center justify-center flex-col;
    .title01 {
      @apply font-bold text-3xl text-gray-800;
    }
    .title02 {
      @apply flex items-center justify-center my-5 text-gray-300 space-x-2;
    }
  }
}

.bg-squares{
  list-style: none;
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 1;
  pointer-events: none;
}
.bg-squares li{
  width: 40px;
  height: 40px;
  background-color: rgba(255, 255, 255, 0.15);
  position: absolute;
  bottom: -160px;
  /* 执行动画：动画名 时长 线性 无限次播放*/
  animation: squres 2s linear infinite ;
}
/* 为每一个方块设置不同的位置、大小、动画延迟时间、动画时长、背景色  */
.bg-squares li:nth-child(1){
  left: 10%;
}
.bg-squares li:nth-child(2){
  left: 20%;
  width: 80px;
  height: 80px;
  /* 动画延迟时间 */
  animation-delay:2s;
  /* 动画时长 */
  animation-duration: 17s;
}
.bg-squares li:nth-child(3){
  left: 25%;
  animation-delay: 4s;
}
.bg-squares li:nth-child(4){
  left: 40%;
  width: 60px;
  height: 60px;
  background-color:rgba(255, 255, 255, 0.25) ;
  animation-duration: 22s;
}
.bg-squares li:nth-child(5){
  left: 70%;
}
.bg-squares li:nth-child(6){
  left: 80%;
  width: 120px;
  height:120px;
  background-color:rgba(255, 255, 255, 0.2) ;
  animation-delay: 3s;
}
.bg-squares li:nth-child(7){
  left: 32%;
  width: 160px;
  height: 160px;
  animation-delay: 7s;
}
.bg-squares li:nth-child(8){
  left: 55%;
  width: 20px;
  height: 20px;
  animation-delay: 15s;
  animation-duration: 40s;
}
.bg-squares li:nth-child(9){
  left: 25%;
  width: 10px;
  height: 10px;
  background-color: rgba(255, 255, 255, 0.3);
  animation-delay: 2s;
  animation-duration: 40s;
}
.bg-squares li:nth-child(10){
  left: 90%;
  width: 160px;
  height: 160px;
  animation-delay: 11s;
}
@keyframes squres{
  0%{
    transform: translateY(0);
  }
  100%{
    transform: translateY(-120vh) rotate(600deg);
  }
}
</style>

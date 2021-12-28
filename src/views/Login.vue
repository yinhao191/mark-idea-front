<template>
  <div style="margin-top: 40px; ">
    <el-card class="box-card" style="background-color: rgb(253, 253, 253)">
      <div align="center">
        <div style="font-size: 40px" class="markideaname">
          &nbsp;&nbsp;&nbsp;&nbsp;
          <strong>{{pageTitle}}</strong>&nbsp;&nbsp;&nbsp;&nbsp;
        </div>
      </div>
      <div align="center" class="box-img">📕</div>

      <el-form :model="formData" @keyup.enter.native="handleLogin">
        <el-form-item prop="username" :rules="formRules.username">
          <el-input maxlength="10" class="box-input" v-model="formData.username" placeholder="用户名" />
        </el-form-item>
        <el-form-item prop="password" :rules="formRules.password">
          <el-input class="box-input" type="password" v-model="formData.password" placeholder="密码" />
        </el-form-item>
        <div class="box-btn">
          <el-button @click="handleLogin" type="primary">登录</el-button>
          <el-button @click="handleRegister">注册</el-button>
        </div>
      </el-form>
    </el-card>
  </div>
</template>
<script>
import axios from "axios";
import global from "../global";

export default {
  name: "login",
  data() {
    return {
      pageTitle: this.$store.getters.getWebsiteTitle,
      formData: {
        username: null,
        password: null
      },
      formRules: {
        username: [
          { required: true, message: "不能为空" },
          { pattern: /^[A-Za-z0-9]+$/, message: "参数非法" }
        ],
        password: [
          { required: true, message: "不能为空" },
          { pattern: /^[A-Za-z0-9!@#$^]+$/, message: "参数非法" }
        ]
      }
    };
  },

  methods: {
    handleLogin() {
      let user = {
        username: this.formData.username,
        password: this.formData.password
      };

      axios.post(global.HOST_URL + "/user/login", user).then(res => {
        res = res.data;
        if (res.code === 0) {
          console.log(res.data);
          this.$notify({
            type: "success",
            message: "登录成功",
            duration: 1000
          });
          this.$store.commit("setLocalInfo", res.data);
          setTimeout(() => {
            this.$router.push("/");
          }, 700);
        } else {
          this.$notify({
            type: "warning",
            message: "用户名或密码错误",
            duration: 2000
          });
        }
      });
    },

    handleRegister() {
      let user = {
        username: this.formData.username,
        password: this.formData.password
      };
      let url = global.HOST_URL + "/user/register";
      axios.post(url, user).then(res => {
        res = res.data;
        if (res.code === 0) {
          this.$notify({
            type: "success",
            message: "注册成功"
          });
        } else {
          this.$notify({
            type: "warning",
            message: res.msg,
            duration: 700
          });
        }
      });
    }
  },
  mounted() {
    document.title = this.$store.getters.getWebsiteTitle + " - 登录";
  }
};
</script>
<style>
.box-btn {
  float: right;
  margin-bottom: 10px;
  margin-top: 10px;
}

.box-card {
  max-width: 380px;
  margin: 0 auto;
}

.box-img {
  margin: 0 auto;
  /* max-height: 400px; */
  width: 260px;
  font-size: 150px;
}
.markideaname {
  min-width: 200px;
  border-bottom-style: solid;
  border-width: 1px;
  border-color: lightgray;
  margin: 10px;
}
</style>
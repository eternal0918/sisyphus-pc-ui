<template>
  <div class="login">
    <div style="display: flex;width: 100%;height: 100%;gap: 40px">
      <!--      左侧banner-->
      <div class="login-banner">

      </div>
      <!--右侧登录-->
      <div style="flex: 1;position: relative">

        <div style="display: flex">
          <img src="../assets/logo/logo.png" alt="" style="height: 100px;">
          <h1 class="login-title">SISYPHUS</h1>
        </div>
        <el-form ref="loginForm" :model="loginForm" :rules="loginRules" class="login-form">
          <h1>您好，欢迎访问！</h1>
          <br>
          <h2 style="color: grey;letter-spacing: 1px;font-size: 15px">请完善个人信息用以访问Sisyphus管理平台</h2>
          <br>
          <el-form-item prop="username">
            <el-input class="input-style"
                      v-model="loginForm.username"
                      type="text"
                      auto-complete="off"
                      placeholder="账号"
            >
              <svg-icon slot="prefix" icon-class="user" class="el-input__icon input-icon"/>
            </el-input>
          </el-form-item>
          <el-form-item prop="password">
            <el-input
              v-model="loginForm.password"
              type="password"
              auto-complete="off"
              placeholder="密码"
              @keyup.enter.native="handleLogin"
            >
              <svg-icon slot="prefix" icon-class="password" class="el-input__icon input-icon"/>
            </el-input>
          </el-form-item>
          <el-form-item prop="code" v-if="captchaEnabled">
            <div style="display: flex">
              <el-input
                v-model="loginForm.code"
                auto-complete="off"
                placeholder="验证码"
                style="flex: 1"
                @keyup.enter.native="handleLogin"
              >
                <svg-icon slot="prefix" icon-class="validCode" class="el-input__icon input-icon"/>
              </el-input>
              <div style="width: 10px"></div>
              <div class="login-code">
                <img :src="codeUrl" @click="getCode" class="login-code-img"/>
              </div>
            </div>
          </el-form-item>
          <el-checkbox v-model="loginForm.rememberMe" style="margin-bottom: 20px">记住密码</el-checkbox>
          <el-form-item style="width:100%;">
            <!--            <el-button-->
            <!--              :loading="loading"-->
            <!--              size="medium"-->
            <!--              type="primary"-->
            <!--              style="width:100%;"-->
            <!--              @click.native.prevent="handleLogin"-->
            <!--            >-->
            <!--              <span v-if="!loading">登 录</span>-->
            <!--              <span v-else>登 录 中...</span>-->
            <!--            </el-button>-->

            <button class="btn" @click="handleLogin">
              <i class="animation"></i>
              <span v-if="!loading">登 录</span>
              <span v-else>登 录 中...</span>
              <i class="animation"></i>
            </button>

            <div style="float: right;" v-if="register">
              <router-link class="link-type" :to="'/register'">立即注册</router-link>
            </div>
          </el-form-item>
          <el-divider><span style="color: darkgrey;font-size: 10px">OR</span></el-divider>
          <el-form-item style="width:100%;">
            <function-button style="margin:0px 0px 20px 0px;"></function-button>
          </el-form-item>
          <!--          <loader-circular type="1"></loader-circular>-->
        </el-form>

        <!--  底部  -->
        <div class="el-login-footer">
          <span>Copyright © 2018-2025 ruoyi.vip All Rights Reserved.</span>
        </div>
      </div>
    </div>

  </div>
</template>

<script>
import {getCodeImg} from "@/api/login";
import Cookies from "js-cookie";
import {encrypt, decrypt} from '@/utils/jsencrypt'
import FunctionButton from "@/components/FunctionButton/index.vue";
import LoaderCircular from "@/components/LoaderCircular/index.vue";

export default {
  name: "Login",
  components: {FunctionButton, LoaderCircular},
  data() {
    return {
      codeUrl: "",
      loginForm: {
        username: "admin",
        password: "admin123",
        rememberMe: false,
        code: "",
        uuid: ""
      },
      loginRules: {
        username: [
          {required: true, trigger: "blur", message: "请输入您的账号"}
        ],
        password: [
          {required: true, trigger: "blur", message: "请输入您的密码"}
        ],
        code: [{required: true, trigger: "change", message: "请输入验证码"}]
      },
      loading: false,
      // 验证码开关
      captchaEnabled: true,
      // 注册开关
      register: false,
      redirect: undefined
    };
  },

  watch: {
    $route: {
      handler: function (route) {
        this.redirect = route.query && route.query.redirect;
      },
      immediate: true
    }
  },
  created() {
    this.getCode();
    this.getCookie();
  },
  methods: {
    getCode() {
      getCodeImg().then(res => {
        this.captchaEnabled = res.captchaEnabled === undefined ? true : res.captchaEnabled;
        if (this.captchaEnabled) {
          this.codeUrl = "data:image/gif;base64," + res.img;
          this.loginForm.uuid = res.uuid;
        }
      });
    },
    getCookie() {
      const username = Cookies.get("username");
      const password = Cookies.get("password");
      const rememberMe = Cookies.get('rememberMe')
      this.loginForm = {
        username: username === undefined ? this.loginForm.username : username,
        password: password === undefined ? this.loginForm.password : decrypt(password),
        rememberMe: rememberMe === undefined ? false : Boolean(rememberMe)
      };
    },
    handleLogin() {
      this.$refs.loginForm.validate(valid => {
        if (valid) {
          this.loading = true;
          if (this.loginForm.rememberMe) {
            Cookies.set("username", this.loginForm.username, {expires: 30});
            Cookies.set("password", encrypt(this.loginForm.password), {expires: 30});
            Cookies.set('rememberMe', this.loginForm.rememberMe, {expires: 30});
          } else {
            Cookies.remove("username");
            Cookies.remove("password");
            Cookies.remove('rememberMe');
          }
          this.$store.dispatch("Login", this.loginForm).then(() => {
            this.$router.push({path: this.redirect || "/"}).catch(() => {
            });
          }).catch(() => {
            this.loading = false;
            if (this.captchaEnabled) {
              this.getCode();
            }
          });
        }
      });
    }
  }
};
</script>

<style lang="scss" scoped>
:deep(.el-input__inner ) {
  background-color: #F4F4F4 !important;
  color: grey !important;
  border: #F4F4F4 solid 1px !important;
  border-radius: 10px !important;
  //padding: 0px 30px!important;
}

:deep(.el-input__inner:focus) {
  color: black !important;
  transition: .3s ease-in-out;
}
</style>

<style rel="stylesheet/scss" lang="scss">


.login {

  height: 100vh;
  width: 100vw;
  padding: 20px;
  box-sizing: border-box;
  //background-image: url("../assets/images/login-background.jpg");
  //background-size: cover;
}

.login-title {
  position: relative;
  font-size: 120px;
  font-weight: bold;
  background: #fff;
  color: #000;
  overflow: hidden;

  &::before,
  &::after {
    content: "";
    position: absolute;
    top: -200px;

    width: 500px;
    height: 500px;
    border-radius: 45% 48% 43% 47%;
    transform: translate(-50%, -50%);
    background: rgba(255, 252, 95, 0.94);
    animation: rotate 10s infinite linear alternate;
    z-index: 1;
    mix-blend-mode: lighten;
  }

  &::after {
    border-radius: 43% 47% 44% 48%;
    animation: rotate 10s infinite .5s linear alternate;
  }
}

@keyframes rotate {
  0% {
    transform: translate(-50%, -50%) rotate(0);
  }
  100% {
    transform: translate(-50%, -50%) rotate(360deg);
  }
}

.login-banner {
  flex: 1;
  height: 100%;
  width: 100%;
  background-image: url("../assets/images/login-background.jpg");
  background-size: cover;
  border-radius: 20px;
}

.title {
  margin: 0px auto 30px auto;
  text-align: center;
  color: #707070;
}

.login-form {
  border-radius: 6px;
  background: #ffffff;
  width: 400px;
  padding: 25px 25px 5px 25px;
  transform: translate(-50%, -50%);
  position: absolute;
  top: 35%;
  left: 35%;
  transform: scale(120%);

  .el-input {
    height: 38px;

    input {
      height: 38px;
    }
  }

  .input-icon {
    height: 39px;
    width: 14px;
    margin-left: 2px;
  }
}

.login-tip {
  font-size: 13px;
  text-align: center;
  color: #bfbfbf;
}

.login-code {

  float: right;

  img {
    cursor: pointer;
    vertical-align: middle;
  }
}


.login-code-img {
  height: 38px;
  transform: translateY(-2px);
  border-radius: 5px;
}

.el-login-footer {
  height: 40px;
  line-height: 40px;
  position: fixed;
  bottom: 10px;
  width: 100%;
  left: 67.5%;
  text-align: left;
  color: grey;
  letter-spacing: 1px;
}


.btn {
  width: 100%;
  outline: 0;
  display: inline-flex;
  align-items: center;
  justify-content: space-between;
  background: black;

  border: 0;
  border-radius: 4px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, .1);
  box-sizing: border-box;
  padding: 10px 20px;
  color: #fff;
  letter-spacing: 1.2px;
  overflow: hidden;
  cursor: pointer;
}

.btn:hover {
  opacity: .95;
}

.btn .animation {
  border-radius: 100%;
  animation: ripple 0.6s linear infinite;
}

@keyframes ripple {
  0% {
    box-shadow: 0 0 0 0 rgba(255, 255, 255, 0.1), 0 0 0 20px rgba(255, 255, 255, 0.1), 0 0 0 40px rgba(255, 255, 255, 0.1), 0 0 0 60px rgba(255, 255, 255, 0.1);
  }

  100% {
    box-shadow: 0 0 0 20px rgba(255, 255, 255, 0.1), 0 0 0 40px rgba(255, 255, 255, 0.1), 0 0 0 60px rgba(255, 255, 255, 0.1), 0 0 0 80px rgba(255, 255, 255, 0);
  }
}
</style>

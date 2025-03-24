<template>
  <div class="login-style">
    <div class="login-container">
      <!--      左侧-->
      <div class="login-left">
        <div class="login-left-form">

          <div style="display: flex;justify-content: space-between;margin-bottom: 100px">
            <h2 style="color: gray">Sisyphus Operation Platform</h2>
            <h2>Sign Up</h2>
          </div>

          <div style="display: flex;flex-direction: column;gap: 25px">
            <!--          其他登录-->
            <div style="display: flex;justify-content: space-between">
              <h1 style="font-size: 50px">Log In</h1>
              <div style="width: 180px">
                <function-button></function-button>
              </div>
            </div>

            <br>
            <!--          登录表单-->
            <div class="form-input-box">
              <div class="form-input-icon"><i class='bx bx-voicemail bx-sm'/></div>
              <input type="text"
                     class="form-input-text"
                     autocomplete="off"
                     v-model="loginForm.username">
            </div>
            <div class="form-input-box">
              <div class="form-input-icon"><i class='bx bx-lock bx-sm'/></div>
              <input type="password"
                     class="form-input-text"
                     placeholder="Password"
                     autocomplete="off"
                     v-model="loginForm.password"/>
              <button class="form-btn-forgot">
                忘记密码
              </button>
            </div>

            <br>
            <div style="display: flex;gap: 25px;align-items: center">
              <span style="font-size: 18px;color: dimgrey;line-height: 1.5;letter-spacing: 1px;flex: 1">
              For use by audits only(18 years of age and older),Keep ouf of reach of children and pets.In case of occidental ingestion contact our <u
                style="cursor: pointer">hotline</u>
            </span>

              <button class="form-btn-login " @click="handleLogin" v-if="!loading">
                <i class='bx bx-chevron-right bx-md'/>
              </button>
              <loader-circular type="1" v-else></loader-circular>
            </div>
            <br>
            <h2 style="text-align: center">Please consume responsibly!</h2>
          </div>


        </div>
        <div class="login-left-card">
          <div>
            <span style="font-size: 60px;margin-bottom: 10px;">New in</span>
            <br>
            <span style="font-size: 40px;color: darkgray">Author.Zxp</span>
          </div>
          <span style="font-size: 40px;text-align: right">Discover</span>
        </div>

      </div>
      <!--      右侧-->
      <div class="login-right">
        <div class="login-right-card">
          <div class="head-foot" style="font-size: 80px;font-weight: bold;">
            <span style="font-size: 110px">Thu</span>
            <span style="color: darkgray">24th</span>
          </div>
          <div class="middle">
            <span>18 PM</span>
            <span>Kerkstraat 12B</span>
            <span>Amsterdam</span>
          </div>
          <div class="head-foot">
            <!--            <img src="../assets/logo/logo.png" alt="" style="height: 70px;">-->
            <i class='bx bx-pyramid bx-lg'></i>
            <div class="author-text">
              Author.Zxp
            </div>
          </div>
        </div>
        <div class="login-right-box">
          <div class="login-right-box-banner">
            <div style="text-align: right" class="login-title">
              <span style="font-size: 40px;">SISYPHUS</span>
              <br>
              <span style="font-size: 25px;">WELCOME !</span>
            </div>
          </div>

          <div style="display: flex;justify-content: right">
            <div class="btn-contain">
              <div class="btn-base login-right-box-btn1 btn-blur"></div>
              <div class="btn-base login-right-box-btn2 btn-blur"></div>
              <div class="btn-base login-right-box-btn1 btn-text">
                <span>Join the community</span>
              </div>
              <div class="btn-base login-right-box-btn2 btn-text" style="z-index: 999;">
                <i class='bx bx-chevron-right bx-md'/>
              </div>
            </div>
          </div>
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
  name: "LoginBlur",
  components: {FunctionButton, LoaderCircular},
  data() {
    return {
      codeUrl: "",
      loginForm: {
        username: "admin",
        password: "admin123",
        rememberMe: false,
        code: "",
        uuid: "",
        expires: 30,
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
    // this.getCode();
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
      this.loading = true;
      if (this.loginForm.rememberMe) {
        Cookies.set("username", this.loginForm.username, {expires: this.loginForm.expires});
        Cookies.set("password", encrypt(this.loginForm.password), {expires: this.loginForm.expires});
        Cookies.set('rememberMe', this.loginForm.rememberMe, {expires: this.loginForm.expires});
      } else {
        Cookies.remove("username");
        Cookies.remove("password");
        Cookies.remove('rememberMe');
      }
      this.$store.dispatch("Login", this.loginForm).then(() => {
        setTimeout(() => {
          this.$router.push({path: this.redirect || "/"}).catch(() => {
          });
        }, 3000)
      });
    }
  }
};
</script>

<style lang="scss" scoped>
@import "src/assets/styles/loginBlur/login";
</style>



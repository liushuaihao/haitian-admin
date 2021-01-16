<template>
  <div id="login-main">
    <!-- login-div-box -->
    <img src="~@/assets/img/title.png" style="margin-bottom:30px" alt="" />
    <div class="login-div-box">
      <div class="l-tab">
        <div class="active">用户登录</div>
      </div>
      <div class="">
        <el-form
          :model="dataForm"
          :rules="dataRule"
          ref="dataForm"
          @keyup.enter.native="dataFormSubmitHandle()"
        >
          <el-form-item prop="username">
            <el-input
              placeholder="手机号"
              v-model="dataForm.username"
            ></el-input>
          </el-form-item>
          <el-form-item prop="password">
            <el-input
              placeholder="密码"
              v-model="dataForm.password"
              show-password
            ></el-input>
          </el-form-item>
          <el-form-item prop="captcha">
            <el-row :gutter="20">
              <el-col :span="14">
                <el-input
                  v-model="dataForm.captcha"
                  placeholder="验证码"
                ></el-input>
              </el-col>
              <el-col :span="10" class="login-captcha" style="padding:0;">
                <img :src="captchaPath" @click="getCaptcha()" />
              </el-col>
            </el-row>
          </el-form-item>
          <el-form-item>
            <el-button
              @click="dataFormSubmitHandle()"
              style="background:#409eff;color:#ffffff;border:none;"
              class="w-percent-100"
              >登录</el-button
            >
          </el-form-item>
        </el-form>

        <div class="login-box-footer">
          <p>
            <span @click="goLink('forgetPassword')">忘记密码</span>
            <span @click="goLink('signIn')">新用户注册</span>
          </p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Cookies from "js-cookie";
import debounce from "lodash/debounce";
import { messages } from "@/i18n";
import { getUUID } from "@/utils";
export default {
  data() {
    return {
      i18nMessages: messages,
      captchaPath: "",
      dataForm: {
        username: "",
        password: "",
        uuid: "",
        captcha: "",
      },
    };
  },
  computed: {
    dataRule() {
      return {
        username: [
          {
            required: true,
            message: this.$t("validate.required"),
            trigger: "blur",
          },
        ],
        password: [
          {
            required: true,
            message: this.$t("validate.required"),
            trigger: "blur",
          },
        ],
        captcha: [
          {
            required: true,
            message: this.$t("validate.required"),
            trigger: "blur",
          },
        ],
      };
    },
  },
  created() {
    this.getCaptcha();
    this.themeColorChangeHandle("default");
  },
  methods: {
    // 获取验证码
    getCaptcha() {
      this.dataForm.uuid = getUUID();
      this.captchaPath = `${window.SITE_CONFIG["apiURL"]}/captcha?uuid=${this.dataForm.uuid}`;
    },
    // 表单提交
    dataFormSubmitHandle: debounce(
      function() {
        this.$refs["dataForm"].validate((valid) => {
          if (!valid) {
            return false;
          }
          this.$http
            .post("/login", this.dataForm)
            .then(({ data: res }) => {
              if (res.code !== 0) {
                this.getCaptcha();
                return this.$message.error(res.msg);
              }
              Cookies.set("token", res.data.token);
              this.$router.replace({ name: "home" });
            })
            .catch(() => {});
        });
      },
      1000,
      { leading: true, trailing: false }
    ),
    themeColorChangeHandle(val) {
      var styleList = [
        {
          id: "J_elementTheme",
          url: `${
            process.env.BASE_URL
          }element-theme/${val}/index.css?t=${new Date().getTime()}`,
        },
        {
          id: "J_auiTheme",
          url: `${
            process.env.BASE_URL
          }element-theme/${val}/aui.css?t=${new Date().getTime()}`,
        },
      ];
      for (var i = 0; i < styleList.length; i++) {
        var el = document.querySelector(`#${styleList[i].id}`);
        if (el) {
          el.href = styleList[i].url;
          continue;
        }
        el = document.createElement("link");
        el.id = styleList[i].id;
        el.href = styleList[i].url;
        el.rel = "stylesheet";
        document.querySelector("head").appendChild(el);
      }
    },

    goLink(name) {
      window.open("#/" + name);
    },
  },
};
</script>

<template>
  <div id="login-main">
    <!-- login-div-box -->
    <img src="~@/assets/img/title.png" style="margin-bottom:30px" alt="" />
    <el-card class="signin">
      <div class="l-tab">
        <div class="active">用户注册</div>
      </div>
      <div class>
        <el-form
          ref="dataForm"
          :model="dataForm"
          :rules="dataRule"
          class="demo-ruleForm"
        >
          <el-form-item prop="realName">
            <el-input placeholder="姓名" v-model="dataForm.realName"></el-input>
          </el-form-item>
          <el-form-item prop="mobile">
            <el-input placeholder="电话" v-model="dataForm.mobile"></el-input>
          </el-form-item>
          <el-form-item prop="idCard">
            <el-input
              placeholder="身份证号"
              v-model="dataForm.idCard"
            ></el-input>
          </el-form-item>
          <el-form-item>
            <el-cascader
              style="width:100%"
              placeholder="地区"
              size="large"
              :options="options"
              v-model="dataForm.selectedOptions"
              @change="handleChange"
            ></el-cascader>
          </el-form-item>

          <el-form-item prop="password">
            <el-input placeholder="密码" v-model="dataForm.password"></el-input>
          </el-form-item>
          <!-- <el-form-item prop="confirmPassword">
            <el-input
              placeholder="确认密码"
              v-model="dataForm.confirmPassword"
            ></el-input>
          </el-form-item> -->
          <el-form-item>
            <el-select
              style="width:100%"
              v-model="dataForm.orgId"
              placeholder="所属机构"
            >
              <el-option label="机构1" value="机构1"></el-option>
              <el-option label="机构2" value="机构2"></el-option>
            </el-select>
          </el-form-item>
          <el-form-item>
            <el-select
              style="width:100%"
              v-model="dataForm.deptId"
              placeholder="所属科室"
            >
              <el-option label="科室1" value="科室1"></el-option>
              <el-option label="科室2" value="科室2"></el-option>
            </el-select>
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
        </el-form>

        <div class="r_button">
          <el-button
            @click="dataFormSubmitHandle()"
            style="background:#409eff;color:#ffffff;border:none;"
            class="w-percent-100"
            >注册</el-button
          >
        </div>
      </div>
    </el-card>
  </div>
</template>

<script>
import { isEmail, isMobile } from "@/utils/validate";
import debounce from "lodash/debounce";
import { messages } from "@/i18n";
import { getUUID } from "@/utils";
export default {
  name: "signIn",
  data() {
    return {
      i18nMessages: messages,
      captchaPath: "",
      dataForm: {
        realName: "", //姓名
        password: "", // 密码,
        idCard: "", // 身份证号
        mobile: "", // 手机号
        cityId: 0, // 所在地区
        // confirmPassword: "", // 确认密码
        deptId: "", // 所属科室
        orgId: "", // 所属机构
        uuid: "",
        captcha: "", // 验证码
      },
      options: [],
    };
  },

  computed: {
    dataRule() {
      var validatePassword = (rule, value, callback) => {
        if (!this.dataForm.id && !/\S/.test(value)) {
          return callback(new Error(this.$t("validate.required")));
        }
        callback();
      };
      var validateConfirmPassword = (rule, value, callback) => {
        if (!this.dataForm.id && !/\S/.test(value)) {
          return callback(new Error(this.$t("validate.required")));
        }
        if (this.dataForm.password !== value) {
          return callback(new Error(this.$t("user.validate.confirmPassword")));
        }
        callback();
      };
      var validateEmail = (rule, value, callback) => {
        if (value && !isEmail(value)) {
          return callback(
            new Error(
              this.$t("validate.format", { attr: this.$t("user.email") })
            )
          );
        }
        callback();
      };
      var validateMobile = (rule, value, callback) => {
        if (value && !isMobile(value)) {
          return callback(
            new Error(
              this.$t("validate.format", { attr: this.$t("user.mobile") })
            )
          );
        }
        callback();
      };
      return {
        realName: [
          {
            required: true,
            message: this.$t("validate.required"),
            trigger: "blur",
          },
        ],
        password: [
          { validator: validatePassword, trigger: "blur" },
          { min: 6, max: 12, message: "请输入6-12位密码", trigger: "blur" },
        ],
        confirmPassword: [
          { validator: validateConfirmPassword, trigger: "blur" },
        ],
        idCard: [
          { required: true, message: "请输入证件号码", trigger: "blur" },
          {
            pattern: /(^[1-9]\d{5}(18|19|([23]\d))\d{2}((0[1-9])|(10|11|12))(([0-2][1-9])|10|20|30|31)\d{3}[0-9Xx]$)|(^[1-9]\d{5}\d{2}((0[1-9])|(10|11|12))(([0-2][1-9])|10|20|30|31)\d{2}$)/,
            message: "证件号码格式有误！",
            trigger: "blur",
          },
        ],
        mobile: [{ validator: validateMobile, trigger: "blur" }],
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
  },
  methods: {
    // 获取验证码
    getCaptcha() {
      this.dataForm.uuid = getUUID();
      this.captchaPath = `${window.SITE_CONFIG["apiURL"]}/captcha?uuid=${this.dataForm.uuid}`;
    },
    handleChange: function(value) {
      console.log(value);
    },
    typeClick(type) {
      this.$emit("typeClick", type);
    },
    // // 地区
    getRegion() {
      this.$http.get("/sys/region/region").then((res) => {
        console.log(res);
      });
    },
    // 表单提交
    dataFormSubmitHandle: debounce(
      function() {
        this.$refs["dataForm"].validate((valid) => {
          if (!valid) {
            return false;
          }
          console.log(this.dataForm);
          this.$http
            .post("/register", this.dataForm)
            .then(({ data: res }) => {
              if (res.code !== 0) {
                this.getCaptcha();
                return this.$message.error(res.msg);
              }
            })
            .catch(() => {});
        });
      },
      1000,
      { leading: true, trailing: false }
    ),
  },
  mounted() {
    // this.getRegion();
  },
};
</script>

<style scoped lang="scss">
.signin {
  width: 400px;
  padding: 5px 10px;
}
.r_title h3 {
  text-align: center;
  line-height: 100px;
}
.r_button {
  display: flex;
}
.r_button > div {
  flex: 1;
  text-align: center;
}
</style>

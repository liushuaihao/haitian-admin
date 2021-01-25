<template>
  <div id="login-main">
    <!-- login-div-box -->
    <img src="~@/assets/img/title.png" style="margin-bottom:30px" alt="" />
    <el-card class="administrator">
      <div class="l-tab">
        <div class="active">联系管理员</div>
      </div>
      <div class="r_form">
        <el-form
          :model="dataForm"
          ref="dataForm"
          :rules="dataRule"
          class="demo-ruleForm"
        >
          <el-form-item prop="realName">
            <el-input
              placeholder="姓名"
              v-model="dataForm.realName"
              clearable
              maxlength="10"
              show-word-limit
            ></el-input>
          </el-form-item>
          <el-form-item prop="mobile">
            <el-input
              placeholder="手机号"
              v-model="dataForm.mobile"
              clearable
              maxlength="11"
              show-word-limit
            ></el-input>
          </el-form-item>
          <el-form-item prop="idCard">
            <el-input
              placeholder="身份证号"
              v-model="dataForm.idCard"
              clearable
              maxlength="20"
              show-word-limit
            ></el-input>
          </el-form-item>
          <el-form-item prop="password">
            <el-input
              placeholder="密码"
              show-password
              v-model="dataForm.password"
              clearable
              maxlength="12"
              show-word-limit
            ></el-input>
          </el-form-item>
          <el-form-item prop="confirmPassword">
            <el-input
              v-model="dataForm.confirmPassword"
              show-password
              type="confirmPassword"
              clearable
              maxlength="12"
              show-word-limit
              placeholder="确认密码"
            ></el-input>
          </el-form-item>
        </el-form>
      </div>
      <div class="r_button">
        <el-button
          @click="dataFormSubmitHandle()"
          style="background:#409eff;color:#ffffff;border:none;"
          class="w-percent-100"
          >提交</el-button
        >
      </div>
      <div class="relationTel">
        <p>联系电话：010-383938</p>
      </div>
    </el-card>
  </div>
</template>

<script>
import debounce from "lodash/debounce";
import { isEmail, isMobile } from "@/utils/validate";
export default {
  data() {
    var checkPhone = (rule, value, callback) => {
      if (!value) {
        return callback(new Error("请输入手机号"));
      } else {
        const reg = /^(((13[0-9]{1})|(14[0-9]{1})|(15[0-9]{1})|(16[0-9]{1})|(18[0-9]{1})|(19[0-9]{1})|(17[0-9]{1}))+\d{8})$/;
        if (reg.test(value)) {
          callback();
        } else {
          return callback(new Error("请输入正确的手机号"));
        }
      }
    };
    return {
      dataForm: {
        realName: "", // 姓名
        mobile: "", // 电话
        idCard: "", // 身份证
        password: "", // 密码
        confirmPassword: "",
      },
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
        realName: [{ required: true, message: "请输入姓名", trigger: "blur" }],
        idCard: [
          { required: true, message: "请输入证件号码", trigger: "blur" },
          {
            pattern: /(^[1-9]\d{5}(18|19|([23]\d))\d{2}((0[1-9])|(10|11|12))(([0-2][1-9])|10|20|30|31)\d{3}[0-9Xx]$)|(^[1-9]\d{5}\d{2}((0[1-9])|(10|11|12))(([0-2][1-9])|10|20|30|31)\d{2}$)/,
            message: "证件号码格式有误！",
            trigger: "blur",
          },
        ],
        password: [
          { required: true, validator: validatePassword, trigger: "blur" },
          { min: 6, max: 12, message: "请输入6-12位密码", trigger: "blur" },
        ],
        confirmPassword: [
          {
            required: true,
            validator: validateConfirmPassword,
            trigger: "blur",
          },
        ],
        mobile: [
          {
            required: true,
            validator: validateMobile,
            trigger: "blur",
          },
        ],
      };
    },
  },
  methods: {
    // 表单提交
    dataFormSubmitHandle: debounce(
      function() {
        this.$refs["dataForm"].validate((valid) => {
          if (!valid) {
            return false;
          }
          this.$http
            .post("/contact", this.dataForm)
            .then(({ data: res }) => {
              if (res.code !== 0) {
                return this.$message.error(res.msg);
              }
              this.$message.success("提交成功");
              this.$router.replace({ name: "login" });
              this.$nextTick(() => {
                this.$refs["dataForm"].resetFields();
              });
            })
            .catch(() => {});
        });
      },
      1000,
      { leading: true, trailing: false }
    ),
  },
};
</script>

<style scoped lang="scss">
.administrator {
  width: 400px;
  padding: 5px 10px;
  border-radius: 6px;
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
.relationTel {
  position: relative;
  bottom: 0;
  right: 0;
  text-align: right;
}
</style>

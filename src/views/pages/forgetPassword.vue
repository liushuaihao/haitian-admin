<template>
  <el-card class="forgetPassword">
    <div class="l-tab">
      <div class="active">找回密码</div>
    </div>
    <div class="r_form">
      <el-form :model="dataForm" ref="dataForm" :rules="dataRule" label-width="100px" class="demo-ruleForm">
        <el-form-item label="手机号" prop="mobile">
          <el-input v-model="dataForm.mobile"></el-input>
        </el-form-item>
        <el-form-item label="密码" prop="password">
          <el-input v-model="dataForm.password" type="password"></el-input>
        </el-form-item>
        <el-form-item label="确认密码" prop="confirmPassword">
          <el-input v-model="dataForm.confirmPassword" type="confirmPassword"></el-input>
        </el-form-item>
        <el-form-item label="验证码" prop="captcha">
          <el-row>
              <el-col :span="14">
                 <el-input v-model="dataForm.captcha" placeholder="验证码"></el-input>
              </el-col>
              <el-col :span="8" :offset="2" style="padding:0;">
                  <el-button  v-if="!sendMsgDisabled"  @click="getCaptcha" >获取验证码</el-button>
                  <el-button  v-if="sendMsgDisabled"  disabled>{{time+'秒后获取'}}</el-button>
              </el-col>
          </el-row>
        </el-form-item>
      </el-form>
    </div>
    <div class="r_button">
      <div>
        <el-button type="primary" @click="dataFormSubmitHandle">登录</el-button>
      </div>
      <div>
        <el-button type="primary" @click="typeClick(0)">取消</el-button>
      </div>
    </div>
    <div class="login-box-footer">
      <p>
        <span @click="typeClick(3)">联系管理员</span>
      </p>
    </div>
  </el-card>
</template>

<script>
import Cookies from 'js-cookie'
import { isEmail, isMobile } from '@/utils/validate'
import debounce from 'lodash/debounce'
export default {
  data () {
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
        mobile: "",
        password: "",
        confirmPassword: "",
        captcha: ""
      },
      time: 60, // 发送验证码倒计时
      sendMsgDisabled: false
    };
  },
  computed: {
    dataRule () {
      var validatePassword = (rule, value, callback) => {
        if (!this.dataForm.id && !/\S/.test(value)) {
          return callback(new Error(this.$t('validate.required')))
        }
        callback()
      }
      var validateConfirmPassword = (rule, value, callback) => {
        if (!this.dataForm.id && !/\S/.test(value)) {
          return callback(new Error(this.$t('validate.required')))
        }
        if (this.dataForm.password !== value) {
          return callback(new Error(this.$t('user.validate.confirmPassword')))
        }
        callback()
      }
      var validateMobile = (rule, value, callback) => {
        if (value && !isMobile(value)) {
          return callback(new Error(this.$t('validate.format', { 'attr': this.$t('user.mobile') })))
        }
        callback()
      }
      return {
        password: [
          { required: true, validator: validatePassword, trigger: 'blur' },
          { min: 6, max: 12, message: "请输入6-12位密码", trigger: "blur" }
        ],
        confirmPassword: [
          { required: true, validator: validateConfirmPassword, trigger: 'blur' }
        ],
        mobile: [
          { required: true, validator: validateMobile, trigger: 'blur' }
        ],
        captcha: [
          { required: true, message: this.$t('validate.required'), trigger: 'blur' }
        ]
      }
    }
  },
  methods: {
    typeClick (type) {
      this.$emit('typeClick', type)
    },
    getCaptcha () {
      let _this = this;
      _this.sendMsgDisabled = true;
      let interval = setInterval(function () {
        if (_this.time-- <= 0) {
          _this.time = 60;
          _this.sendMsgDisabled = false;
          clearInterval(interval);
        }
      }, 1000)
    },
    // 表单提交
    dataFormSubmitHandle: debounce(function () {
      this.$refs['dataForm'].validate((valid) => {
        if (!valid) {
          return false
        }
        console.log(this.dataForm)
      })
    }, 1000, { 'leading': true, 'trailing': false }),
  }
};
</script>

<style scoped lang="scss">
.forgetPassword{
  width: 500px;
  padding: 5px 10px;
  border-radius: 6px
}
.r_button {
  padding-top: 20px;
  display: flex;
}
.r_button > div {
  flex: 1;
  text-align: center;
}
</style>

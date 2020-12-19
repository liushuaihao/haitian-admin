<template>
  <el-card class="forgetPassword">
    <div class="l-tab">
      <div class="active">找回密码</div>
    </div>
    <div class="r_form">
      <el-form ref="form" :model="form" :rules="rules" label-width="100px" class="demo-ruleForm">
        <el-form-item label="电话：" prop="tel">
          <el-input v-model="form.tel"></el-input>
        </el-form-item>
        <el-form-item label="密码：" prop="pass">
          <el-input v-model="form.pass"></el-input>
        </el-form-item>
        <el-form-item label="确认密码：" prop="confirmPass">
          <el-input v-model="form.confirmPass"></el-input>
        </el-form-item>
        <el-form-item label="验证码" prop="verification">
          <el-input v-model="form.verification"></el-input>
        </el-form-item>
      </el-form>
    </div>
    <div class="r_button">
      <div>
        <el-button type="primary">登录</el-button>
      </div>
      <div>
        <el-button type="primary" @click="typeClick(0)">取消</el-button>
      </div>
    </div>
  </el-card>
</template>

<script>
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
      form: {
        tel: "",
        pass: "",
        confirmPass: "",
        verification: "",

      },
      rules: {
        tel: [
          {
            required: true,
            validator: checkPhone,
            trigger: "blur"
          }
        ],
        pass: [
          { required: true, message: "请输入密码", trigger: "blur" },
          { min: 6, max: 12, message: "请输入6-12位密码", trigger: "blur" }
        ],
        confirmPass: [
          { required: true, message: "请输入密码", trigger: "blur" },
          { min: 6, max: 12, message: "请输入6-12位密码", trigger: "blur" }
        ],
        verification: [
          { required: true, message: "请输入验证码", trigger: "blur" },
        ]
      },

    };
  },

  methods: {
    typeClick (type) {
      this.$emit('typeClick', type)
    }
  }
};
</script>

<style scoped lang="scss">
.forgetPassword{
  width: 400px;
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

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
          ref="form"
          :model="form"
          :rules="rules"
          label-width="120px"
          class="demo-ruleForm"
        >
          <el-form-item label="用户名：" prop="userName">
            <el-input v-model="form.userName"></el-input>
          </el-form-item>
          <el-form-item label="姓名：" prop="name">
            <el-input v-model="form.name"></el-input>
          </el-form-item>
          <el-form-item label="电话：" prop="tel">
            <el-input v-model="form.tel"></el-input>
          </el-form-item>
          <el-form-item label="身份证号：" prop="identityCard">
            <el-input v-model="form.identityCard"></el-input>
          </el-form-item>
          <el-form-item label="密码：" prop="pass">
            <el-input v-model="form.pass"></el-input>
          </el-form-item>
          <el-form-item label="再次输入密码：" prop="confirmPass">
            <el-input v-model="form.confirmPass"></el-input>
          </el-form-item>
        </el-form>
      </div>
      <div class="r_button">
        <div>
          <el-button type="primary">提交</el-button>
        </div>
        <div>
          <el-button type="primary" @click="typeClick(0)">取消</el-button>
        </div>
      </div>
      <div class="relationTel">
        <p>联系电话：010-383938</p>
      </div>
    </el-card>
  </div>
</template>

<script>
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
      form: {
        userName: "", // 用户名
        name: "", // 姓名
        tel: "", // 电话
        identityCard: "", // 身份证
        pass: "", // 密码
        confirmPass: "", // 确认密码
      },
      rules: {
        userName: [
          { required: true, message: "请输入用户名", trigger: "blur" },
        ],
        name: [{ required: true, message: "请输入姓名", trigger: "blur" }],
        tel: [
          {
            required: true,
            validator: checkPhone,
            trigger: "blur",
          },
        ],
        identityCard: [
          { required: true, message: "请输入证件号码", trigger: "blur" },
          {
            pattern: /(^[1-9]\d{5}(18|19|([23]\d))\d{2}((0[1-9])|(10|11|12))(([0-2][1-9])|10|20|30|31)\d{3}[0-9Xx]$)|(^[1-9]\d{5}\d{2}((0[1-9])|(10|11|12))(([0-2][1-9])|10|20|30|31)\d{2}$)/,
            message: "证件号码格式有误！",
            trigger: "blur",
          },
        ],
        pass: [
          { required: true, message: "请输入密码", trigger: "blur" },
          { min: 6, max: 12, message: "请输入6-12位密码", trigger: "blur" },
        ],
        confirmPass: [
          { required: true, message: "请输入密码", trigger: "blur" },
          { min: 6, max: 12, message: "请输入6-12位密码", trigger: "blur" },
        ],
      },
    };
  },
  methods: {
    typeClick(type) {
      this.$emit("typeClick", type);
    },
  },
};
</script>

<style scoped lang="scss">
.administrator {
  width: 600px;
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

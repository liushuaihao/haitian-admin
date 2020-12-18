<template>
  <div>
    <div class="r_title">
      <h3>用户注册</h3>
    </div>
    <div class="r_form">
      <el-form ref="form" :model="form" :rules="rules" label-width="100px" class="demo-ruleForm">
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
        <el-form-item label="角色：" prop="region">
          <el-select v-model="form.region" placeholder="请选择活动区域">
            <el-option label="管理员" value="管理员"></el-option>
            <el-option label="医生" value="医生"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="地区：">
          <el-cascader
            size="large"
            :options="options"
            v-model="selectedOptions"
            @change="handleChange"
          ></el-cascader>
        </el-form-item>

        <el-form-item label="密码：" prop="pass">
          <el-input v-model="form.pass"></el-input>
        </el-form-item>
        <el-form-item label="确认密码：" prop="confirmPass">
          <el-input v-model="form.confirmPass"></el-input>
        </el-form-item>
        <el-form-item label="所属机构：" v-if="form.region=='医生'">
          <el-select v-model="form.organization" placeholder="请选择活动区域">
            <el-option label="机构1" value="机构1"></el-option>
            <el-option label="机构2" value="机构2"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="所属科室：" v-if="form.region=='医生'">
          <el-select v-model="form.administrative" placeholder="请选择活动区域">
            <el-option label="科室1" value="科室1"></el-option>
            <el-option label="科室2" value="科室2"></el-option>
          </el-select>
        </el-form-item>
      </el-form>
    </div>
    <div class="r_button">
      <div>
        <el-button type="primary" @click="register()">注册</el-button>
      </div>
      <div>
        <el-button type="primary">取消</el-button>
      </div>
    </div>
  </div>
</template>

<script>
import { regionData } from "element-china-area-data";
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
        userName: "", //用户名
        name: "", //姓名
        tel: "", //电话
        identityCard: "", //身份证
        region: "", //角色
        pass: "", //密码
        confirmPass: "", //确认密码
        organization:"",  //所属机构
        administrative:"", //所属科室

      },
      rules: {
        userName: [
          { required: true, message: "请输入用户名", trigger: "blur" }
        ],
        name: [{ required: true, message: "请输入姓名", trigger: "blur" }],
        tel: [
          {
            required: true,
            validator: checkPhone,
            trigger: "blur"
          }
        ],
        identityCard: [
          { required: true, message: "请输入证件号码", trigger: "blur" },
          {
            pattern: /(^[1-9]\d{5}(18|19|([23]\d))\d{2}((0[1-9])|(10|11|12))(([0-2][1-9])|10|20|30|31)\d{3}[0-9Xx]$)|(^[1-9]\d{5}\d{2}((0[1-9])|(10|11|12))(([0-2][1-9])|10|20|30|31)\d{2}$)/,
            message: "证件号码格式有误！",
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
        region: [{ required: true }]
      },
      options: regionData,
      selectedOptions: []
    };
  },

  methods: {
    register: function() {
      console.log(this.form.region);
    },
    handleChange: function(value) {
      console.log(value);
    }
  }
};
</script>

<style scoped lang="scss">
* {
  margin: 0;
  padding: 0;
}
.r_title h3 {
  text-align: center;
  line-height: 100px;
}
.r_form {
  padding-left: 160px;
}
.r_button {
  display: flex;
}
.r_button > div {
  flex: 1;
  text-align: center;
}
</style>
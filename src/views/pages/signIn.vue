<template>
  <div id="login-main">
    <!-- login-div-box -->
    <img src="~@/assets/img/title.png" style="margin-bottom:30px;" alt="" />
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
              clearable
              v-model="dataForm.idCard"
              maxlength="20"
              show-word-limit
            ></el-input>
          </el-form-item>
          <el-form-item>
            <!-- <ren-region-tree
              v-model="dataForm.cityId"
              placeholder="选择区域"
            ></ren-region-tree> -->
            <el-cascader
              style="width:100%;"
              v-model="dataForm.cityId"
              placeholder="选择区域"
              :options="regionTree"
              :props="optionsProps"
              @change="handleChange"
              clearable
            ></el-cascader>
          </el-form-item>

          <el-form-item prop="password">
            <el-input
              show-password
              placeholder="密码"
              clearable
              v-model="dataForm.password"
              maxlength="12"
            ></el-input>
          </el-form-item>
          <el-form-item prop="confirmPassword">
            <el-input
              show-password
              placeholder="确认密码"
              clearable
              v-model="dataForm.confirmPassword"
              maxlength="12"
            ></el-input>
          </el-form-item>

          <el-form-item prop="orgId">
            <el-select
              style="width:100%"
              clearable
              v-model="dataForm.orgId"
              placeholder="所属机构"
            >
              <el-option
                v-for="organ in organList"
                :key="organ.id"
                :label="organ.orgName"
                :value="organ.id"
              ></el-option>
            </el-select>
          </el-form-item>
          <el-form-item prop="deptId">
            <el-select
              style="width:100%"
              clearable
              v-model="dataForm.deptId"
              placeholder="所属科室"
            >
              <el-option
                v-for="dept in deptList"
                :key="dept.id"
                :label="dept.deptName"
                :value="dept.id"
              ></el-option>
            </el-select>
          </el-form-item>
          <el-form-item prop="captcha">
            <el-row :gutter="20">
              <el-col :span="14">
                <el-input
                  v-model="dataForm.captcha"
                  placeholder="验证码"
                  clearable
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
import { treeDataTranslate } from "@/utils";
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
      deptList: [],
      organList: [],
      optionsProps: {
        value: "id",
        label: "name",
        children: "children",
      },
      regionTree: [],
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
    this.themeColorChangeHandle("default");
    this.getCaptcha();
    this.getDeptList();
    this.getOrganList();
    this.regionTreeList();
  },
  methods: {
    // 获取角色列表 科室管理
    getDeptList() {
      return this.$http
        .get("/sys/dept/list/all")
        .then(({ data: res }) => {
          if (res.code !== 0) {
            return this.$message.error(res.msg);
          }
          this.deptList = res.data;
        })
        .catch(() => {});
    },
    // 获取角色列表 科室管理
    getOrganList() {
      return this.$http
        .get("/sys/org/list/all")
        .then(({ data: res }) => {
          if (res.code !== 0) {
            return this.$message.error(res.msg);
          }
          this.organList = res.data;
        })
        .catch(() => {});
    },
    handleChange(value) {
      this.dataForm.cityId = value[value.length - 1];
    },
    regionTreeList() {
      this.$http
        .get("/sys/region/tree")
        .then(({ data: res }) => {
          if (res.code !== 0) {
            return this.$message.error(res.msg);
          }
          let regionTree = treeDataTranslate(res.data);
          this.regionTree = this.delateChildren(regionTree);
          console.log(this.regionTree);
        })
        .catch(() => {});
    },
    delateChildren(arr) {
      if (arr.length) {
        for (let i in arr) {
          if (arr[i].children.length) {
            this.delateChildren(arr[i].children);
          } else {
            delete arr[i].children;
          }
        }
      }
      return arr;
    },
    // 获取验证码
    getCaptcha() {
      this.dataForm.uuid = getUUID();
      this.captchaPath = `${window.SITE_CONFIG["apiURL"]}/captcha?uuid=${this.dataForm.uuid}`;
    },
    typeClick(type) {
      this.$emit("typeClick", type);
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

              this.$message.success("注册成功");
              this.$router.replace({ name: "login" });
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

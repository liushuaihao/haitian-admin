<template>
  <el-dialog
    :visible.sync="visible"
    :title="!dataForm.id ? $t('add') : $t('update')"
    :close-on-click-modal="false"
    :close-on-press-escape="false"
  >
    <el-form
      :model="dataForm"
      :rules="dataRule"
      ref="dataForm"
      @keyup.enter.native="dataFormSubmitHandle()"
      label-width="120px"
    >
      <el-form-item prop="realName" :label="$t('user.realName')">
        <el-input
          v-model="dataForm.realName"
          :placeholder="$t('user.realName')"
        ></el-input>
      </el-form-item>
      <el-form-item prop="gender" :label="$t('user.gender')">
        <ren-radio-group
          v-model="dataForm.gender"
          dict-type="gender"
        ></ren-radio-group>
      </el-form-item>
      <el-form-item prop="mobile" :label="$t('user.mobile')">
        <el-input
          v-model="dataForm.mobile"
          :placeholder="$t('user.mobile')"
        ></el-input>
      </el-form-item>
      <el-form-item prop="idCard" label="身份证号">
        <el-input v-model="dataForm.idCard" placeholder="身份证号"></el-input>
      </el-form-item>
      <el-form-item prop="userType" label="角色" class="role-list">
        <el-select
          v-model="dataForm.userType"
          :placeholder="$t('user.roleIdList')"
        >
          <el-option
            v-for="role in roleList"
            :key="role.id"
            :label="role.name"
            :value="role.id"
          ></el-option>
        </el-select>
      </el-form-item>
      <el-form-item
        prop="deptId"
        label="所属科室"
        v-if="dataForm.userType == 1"
        class="role-list"
      >
        <el-select v-model="dataForm.deptId" placeholder="所属科室">
          <el-option
            v-for="dept in deptList"
            :key="dept.id"
            :label="dept.deptName"
            :value="dept.id"
          ></el-option>
        </el-select>
      </el-form-item>
      <el-form-item
        v-if="dataForm.userType == 1"
        prop="orgId"
        label="所属机构"
        class="role-list"
      >
        <el-select v-model="dataForm.orgId" placeholder="所属机构">
          <el-option
            v-for="organ in organList"
            :key="organ.id"
            :label="organ.orgName"
            :value="organ.id"
          ></el-option>
        </el-select>
      </el-form-item>
      <el-form-item prop="cityId" label="所在地区">
        <!-- <ren-region-tree
          v-model="dataForm.cityId"
          placeholder="选择区域"
          :parent-name.sync="dataForm.cityName"
        ></ren-region-tree> -->
        <el-cascader
          v-if="visible"
          style="width:250px;"
          v-model="dataForm.cityId"
          placeholder="选择区域"
          :options="regionTree"
          :props="optionsProps"
          @change="handleChange"
        ></el-cascader>
      </el-form-item>
      <el-form-item prop="address" label="详细地址">
        <el-input v-model="dataForm.address" placeholder="详细地址"></el-input>
      </el-form-item>
      <el-form-item prop="introduce" label="个人介绍">
        <el-input
          type="textarea"
          v-model="dataForm.introduce"
          placeholder="个人介绍"
        ></el-input>
      </el-form-item>
    </el-form>
    <template slot="footer">
      <el-button @click="visible = false">{{ $t("cancel") }}</el-button>
      <el-button type="primary" @click="dataFormSubmitHandle()">{{
        $t("confirm")
      }}</el-button>
    </template>
  </el-dialog>
</template>

<script>
import debounce from "lodash/debounce";
import { treeDataTranslate } from "@/utils";
import { isEmail, isMobile, isIdCard } from "@/utils/validate";
export default {
  props: {
    regionTree: {
      type: [Array, Object],
      default: () => [],
    },
  },
  data() {
    return {
      visible: false,
      deptList: [],
      organList: [],
      roleList: [
        { id: "0", name: "管理员" },
        { id: "1", name: "医生" },
        { id: "2", name: "患者" },
        { id: "3", name: "家属" },
      ],
      roleIdListDefault: [],
      dataForm: {
        id: "",
        realName: "",
        idCard: "",
        userType: "",
        deptId: "",
        orgId: "",
        cityId: "",
        gender: 0,
        address: "",
        mobile: "",
        introduce: "",
      },
      optionsProps: {
        value: "id",
        label: "name",
        children: "children",
      },
    };
  },
  computed: {
    dataRule() {
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
      var validateIdCard = (rule, value, callback) => {
        if (value && !isIdCard(value)) {
          return callback(
            new Error(this.$t("validate.format", { attr: "证件号格式错误" }))
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
        mobile: [{ validator: validateMobile, trigger: "blur" }],
        idCard: [{ validator: validateIdCard, trigger: "blur" }],
      };
    },
  },
  methods: {
    init() {
      this.visible = true;
      this.dataForm.deptId = "";
      this.$nextTick(() => {
        this.$refs["dataForm"].resetFields();
        this.roleIdListDefault = [];
        Promise.all([this.getDeptList(), this.getOrganList()]).then(() => {
          if (this.dataForm.id) {
            this.getInfo();
          }
        });
      });
    },
    // 获取角色列表 科室管理
    getDeptList() {
      return this.$http
        .get("/sys/dept/list")
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
        .get("/sys/org/list")
        .then(({ data: res }) => {
          if (res.code !== 0) {
            return this.$message.error(res.msg);
          }
          this.organList = res.data;
        })
        .catch(() => {});
    },
    // 获取信息
    getInfo() {
      this.$http
        .get(`/sys/user/get?id=${this.dataForm.id}`)
        .then(({ data: res }) => {
          if (res.code !== 0) {
            return this.$message.error(res.msg);
          }
          this.dataForm = {
            ...this.dataForm,
            ...res.data,
          };
          // 角色配置, 区分是否为默认角色
          for (var i = 0; i < res.data.roleIdList.length; i++) {
            if (
              this.deptList.filter(
                (item) => item.id === res.data.roleIdList[i]
              )[0]
            ) {
              this.dataForm.roleIdList.push(res.data.roleIdList[i]);
              continue;
            }
            this.roleIdListDefault.push(res.data.roleIdList[i]);
          }
        })
        .catch(() => {});
    },

    handleChange(value) {
      this.dataForm.cityId = value[value.length - 1];
    },
    // 表单提交
    dataFormSubmitHandle: debounce(
      function() {
        this.$refs["dataForm"].validate((valid) => {
          if (!valid) {
            return false;
          }
          this.$http
            .post(
              this.insert ? "/sys/user/add" : "/sys/user/edit",
              this.dataForm
            )
            .then(({ data: res }) => {
              if (res.code !== 0) {
                return this.$message.error(res.msg);
              }
              this.$message({
                message: this.$t("prompt.success"),
                type: "success",
                duration: 500,
                onClose: () => {
                  this.visible = false;
                  this.$emit("refreshDataList");
                },
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

<style lang="scss">
.mod-sys__user {
  .role-list {
    .el-select {
      width: 100%;
    }
  }
}
</style>

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
      <el-form-item prop="orgName" :label="'机构' + $t('dept.name')">
        <el-input
          v-model="dataForm.orgName"
          :placeholder="'机构' + $t('dept.name')"
        ></el-input>
      </el-form-item>
      <el-form-item prop="telephone" :label="$t('sms.mobile')">
        <el-input
          v-model="dataForm.telephone"
          :placeholder="$t('sms.mobile')"
        ></el-input>
      </el-form-item>
      <el-form-item prop="sort" label="所属地域"> </el-form-item>
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
export default {
  data() {
    return {
      visible: false,
      dataForm: {
        id: "",
        orgName: "",
        telephone: "",
        cityId: null,
      },
    };
  },
  computed: {
    dataRule() {
      return {
        orgName: [
          {
            required: true,
            message: this.$t("validate.required"),
            trigger: "blur",
          },
        ],
        telephone: [
          {
            required: true,
            message: this.$t("validate.required"),
            trigger: "change",
          },
        ],
      };
    },
  },
  methods: {
    init() {
      this.visible = true;
      this.$nextTick(() => {
        this.$refs["dataForm"].resetFields();
        if (this.dataForm.id) {
          this.getInfo();
        }
      });
    },
    handleChange(value) {
      console.log(value);
    },
    // 获取信息
    getInfo() {
      this.$http
        .get(`/sys/org/get`, { params: { id: this.dataForm.id } })
        .then(({ data: res }) => {
          if (res.code !== 0) {
            return this.$message.error(res.msg);
          }
          this.dataForm = {
            ...this.dataForm,
            ...res.data,
          };
        })
        .catch(() => {});
    },
    // 表单提交
    dataFormSubmitHandle: debounce(
      function() {
        this.$refs["dataForm"].validate((valid) => {
          if (!valid) {
            return false;
          }
          this.$http["post"](
            !this.dataForm.id ? "/sys/org/add" : "/sys/org/edit",
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
.mod-sys__dept {
  .dept-list {
    .el-input__inner,
    .el-input__suffix {
      cursor: pointer;
    }
  }
}
</style>

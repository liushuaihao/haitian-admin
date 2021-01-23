<template>
  <el-dialog
    :visible.sync="visible"
    :title="'查看'"
    :close-on-click-modal="false"
    :close-on-press-escape="false"
  >
    <el-form :model="dataForm" ref="dataForm" label-width="120px">
      <el-form-item prop="realName" :label="$t('user.realName')">
        <el-input
          v-model="dataForm.realName"
          :placeholder="$t('user.realName')"
          clearable
        ></el-input>
      </el-form-item>
      <el-form-item prop="mobile" :label="$t('user.mobile')">
        <el-input
          v-model="dataForm.mobile"
          :placeholder="$t('user.mobile')"
          clearable
        ></el-input>
      </el-form-item>
      <el-form-item prop="idCard" label="身份证号">
        <el-input
          v-model="dataForm.idCard"
          placeholder="身份证号"
          clearable
        ></el-input>
      </el-form-item>
      <el-form-item prop="status" label="状态">
        <el-tag v-if="dataForm.status === 0" size="small"> 待审核</el-tag>
        <el-tag v-if="dataForm.status === 1" size="small" type="success"
          >通过</el-tag
        >
        <el-tag v-if="dataForm.status === 2" size="small" type="danger"
          >拒绝</el-tag
        >
      </el-form-item>
    </el-form>
    <template slot="footer">
      <el-button @click="visible = false">{{ $t("cancel") }}</el-button>
    </template>
  </el-dialog>
</template>

<script>
export default {
  data() {
    return {
      visible: false,
      dataForm: {
        id: "",
      },
    };
  },
  methods: {
    init() {
      this.visible = true;
      this.dataForm.deptId = "";
      this.$nextTick(() => {
        this.$refs["dataForm"].resetFields();
        if (this.dataForm.id) {
          this.getInfo();
        }
      });
    },

    // 获取信息
    getInfo() {
      this.$http
        .get(`/manage/review/getReviewDetail?id=${this.dataForm.id}`)
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
  },
};
</script>

<style lang="scss"></style>

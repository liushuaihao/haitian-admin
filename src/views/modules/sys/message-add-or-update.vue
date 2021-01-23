<template>
  <el-dialog
    :visible.sync="visible"
    :title="!dataForm.id ? $t('add') : $t('message.detail')"
    :close-on-click-modal="false"
    :close-on-press-escape="false"
  >
    <el-form
      :model="dataForm"
      ref="dataForm"
      :rules="dataRule"
      @keyup.enter.native="dataFormSubmitHandle()"
      label-width="120px"
    >
      <el-form-item prop="noticeTitle" :label="$t('message.messageName')">
        <el-input
          v-if="!dataForm.id"
          v-model="dataForm.noticeTitle"
          :placeholder="$t('message.messageName')"
          clearable
        ></el-input>
        <span v-else>{{ dataForm.noticeTitle }}</span>
      </el-form-item>
      <el-form-item prop="noticeObject" :label="$t('message.sendPeople')">
        <el-select
          v-if="!dataForm.id"
          v-model="dataForm.noticeObject"
          placeholder="请选择"
          clearable
        >
          <el-option
            v-for="item in options"
            :key="item.label"
            :label="item.label"
            :value="item.value"
          >
          </el-option>
        </el-select>
        <span v-if="dataForm.id && dataForm.noticeObject">{{
          options[dataForm.noticeObject].label
        }}</span>
      </el-form-item>
      <el-form-item prop="noticeContent" :label="$t('message.sendMsg')">
        <el-input
          v-if="!dataForm.id"
          type="textarea"
          v-model="dataForm.noticeContent"
          :placeholder="$t('message.sendMsg')"
          clearable
        ></el-input>
        <span v-else>{{ dataForm.noticeContent }}</span>
      </el-form-item>
    </el-form>
    <template slot="footer">
      <el-button @click="visible = false">{{ $t("cancel") }}</el-button>
      <el-button
        type="primary"
        v-if="!dataForm.id"
        @click="dataFormSubmitHandle()"
        >{{ $t("confirm") }}</el-button
      >
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
        noticeTitle: "",
        noticeObject: "",
        noticeContent: "",
      },
      options: [
        {
          value: "0",
          label: "所有用户",
        },
        {
          value: "1",
          label: "医生",
        },
        {
          value: "2",
          label: "患者",
        },
        {
          value: "3",
          label: "患者及家属",
        },
      ],
    };
  },
  computed: {
    dataRule() {
      return {
        noticeTitle: [
          {
            required: true,
            message: this.$t("validate.required"),
            trigger: "blur",
          },
        ],
        noticeObject: [
          {
            required: true,
            message: this.$t("validate.required"),
            trigger: "change",
          },
        ],
        noticeContent: [
          {
            required: true,
            message: this.$t("validate.required"),
            trigger: "blur",
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
    // 获取信息
    getInfo() {
      this.$http
        .get(`/notice/getNoticeDetail?id=${this.dataForm.id}`)
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
          this.$http["post"]("notice/add", this.dataForm)
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
<style lang="scss" scoped>
.el-select {
  width: 100%;
}
</style>

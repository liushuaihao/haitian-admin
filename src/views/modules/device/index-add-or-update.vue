<template>
  <el-dialog
    :visible.sync="visible"
    :title="this.dataForm.id ? '设备详情' : '添加设备'"
    :close-on-click-modal="false"
    :close-on-press-escape="false"
  >
    <el-form :model="dataForm" ref="dataForm" label-width="120px">
      <el-form-item label="账户">
        <el-input v-model="dataForm.name"></el-input>
      </el-form-item>
      <el-form-item label="姓名">
        <el-input v-model="dataForm.realName"></el-input>
      </el-form-item>
      <el-form-item label="年龄">
        <el-input v-model="dataForm.mobile"></el-input>
      </el-form-item>
      <el-form-item label="性别">
        <el-input v-model="dataForm.mobile"></el-input>
      </el-form-item>
      <el-form-item label="手机号">
        <el-input v-model="dataForm.mobile"></el-input>
      </el-form-item>
      <el-form-item label="身份证号">
        <el-input v-model="dataForm.mobile"></el-input>
      </el-form-item>
      <el-form-item label="已绑定设备ID">
        <el-input v-model="dataForm.mobile"></el-input>
      </el-form-item>
      <el-form-item label="GPS定位">
        <el-input v-model="dataForm.mobile"></el-input>
      </el-form-item>
      <p>所附带其他测量设备详情</p>
      <el-form-item label="设备通信号">
        <el-input v-model="dataForm.mobile"></el-input>
      </el-form-item>
      <el-form-item label="心电设备">
        <el-switch v-model="dataForm.xd"> </el-switch>
      </el-form-item>
      <el-form-item label="呼吸设备">
        <el-switch v-model="dataForm.hx"> </el-switch>
      </el-form-item>
      <el-form-item label="上肢设备">
        <el-switch v-model="dataForm.sz"> </el-switch>
      </el-form-item>
      <el-form-item label="下肢设备">
        <el-switch v-model="dataForm.xz"> </el-switch>
      </el-form-item>
      <el-form-item label="中控电量">
        <el-input v-model="dataForm.mobile"></el-input>
      </el-form-item>
      <el-form-item label="上肢设备电量">
        <el-input v-model="dataForm.mobile"></el-input>
      </el-form-item>
      <el-form-item label="下肢设备电量">
        <el-input v-model="dataForm.mobile"></el-input>
      </el-form-item>
    </el-form>
    <template slot="footer">
      <el-button type="primary" @click="visible = false">返回</el-button>
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
        .get(`/device/devicebin/getDeviceBindDetail`, {
          params: { id: this.dataForm.id },
        })
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

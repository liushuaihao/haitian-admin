<template>
  <el-dialog
    :visible.sync="visible"
    :title="this.dataForm.id ? '设备详情' : '添加设备'"
    :close-on-click-modal="false"
    :close-on-press-escape="false"
  >
    <el-form :model="dataForm" ref="dataForm" label-width="120px" disabled>
      <el-form-item label="姓名">
        <el-input v-model="dataForm.realName"></el-input>
      </el-form-item>
      <el-form-item label="年龄">
        <el-input v-model="dataForm.age"></el-input>
      </el-form-item>
      <el-form-item prop="gender" :label="$t('user.gender')">
        <el-input :value="dataForm.gender == 0 ? '男' : '女'"></el-input>
      </el-form-item>
      <el-form-item label="手机号">
        <el-input v-model="dataForm.mobile"></el-input>
      </el-form-item>
      <el-form-item label="身份证号">
        <el-input v-model="dataForm.idCard"></el-input>
      </el-form-item>
      <el-form-item label="已绑定设备ID">
        <el-input v-model="dataForm.deviceId"></el-input>
      </el-form-item>
      <el-form-item label="GPS定位">
        <el-input v-model="dataForm.cityName"></el-input>
      </el-form-item>
      <p>所附带其他测量设备详情</p>
      <el-form-item label="设备通信号">
        {{ dataForm.communication_number }}
        <el-input v-model="dataForm.communication_number"></el-input>
      </el-form-item>
      <el-form-item label="心电设备">
        <el-switch v-model="dataForm.ecg" active-value="1" inactive-value="0">
        </el-switch>
      </el-form-item>
      <el-form-item label="呼吸设备">
        <el-switch
          v-model="dataForm.breathe"
          active-value="1"
          inactive-value="0"
        >
        </el-switch>
      </el-form-item>

      <!-- leftLower	左下肢 0无 1有	string	
    leftUpper	左上肢 0无 1有	string	
    left_lower_voltage	左下肢设备电压	string	
    left_upper_voltage	左上肢设备电压 -->

      <el-form-item label="左上肢设备">
        <el-switch
          v-model="dataForm.leftUpper"
          active-value="1"
          inactive-value="0"
        >
        </el-switch>
      </el-form-item>
      <el-form-item label="左下肢设备">
        <el-switch
          v-model="dataForm.leftLower"
          active-value="1"
          inactive-value="0"
        >
        </el-switch>
      </el-form-item>

      <el-form-item label="右上肢设备">
        <el-switch
          v-model="dataForm.rightUpper"
          active-value="1"
          inactive-value="0"
        >
        </el-switch>
      </el-form-item>
      <el-form-item label="右下肢设备">
        <el-switch
          v-model="dataForm.rightLower"
          active-value="1"
          inactive-value="0"
        >
        </el-switch>
      </el-form-item>
      <!-- rightLower	右下肢 0无 1有	string	
            rightUpper	右上肢 0无 1有	string	
            right_lower_voltage	右下肢设备电压	string	
            right_upper_voltage	右上肢设备电压 -->

      <el-form-item label="中控电量">
        <el-input v-model="dataForm.percentage"
          ><template slot="append">%</template></el-input
        >
      </el-form-item>
      <el-form-item label="左上肢设备电量">
        <el-input v-model="dataForm.left_upper_voltage">
          <template slot="append">%</template></el-input
        >
      </el-form-item>
      <el-form-item label="左下肢设备电量">
        <el-input v-model="dataForm.left_lower_voltage"
          ><template slot="append">%</template></el-input
        >
      </el-form-item>

      <el-form-item label="右上肢设备电量">
        <el-input v-model="dataForm.right_upper_voltage"
          ><template slot="append">%</template></el-input
        >
      </el-form-item>
      <el-form-item label="右下肢设备电量">
        <el-input v-model="dataForm.right_lower_voltage"
          ><template slot="append">%</template></el-input
        >
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

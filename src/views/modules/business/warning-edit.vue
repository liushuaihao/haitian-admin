<template>
  <el-dialog
    :visible.sync="visible"
    title="设置"
    :close-on-click-modal="false"
    :close-on-press-escape="false"
  >
    <el-form
      :model="dataForm"
      :rules="dataRule"
      ref="dataForm"
      label-width="120px"
    >
      <el-form-item prop="name" :label="dataForm.name">
        <span>{{dataForm.telephone}}</span>
      </el-form-item>
      <el-form-item  label="">
        <el-row>
          <el-col :span="12">最小值</el-col>
          <el-col :span="12">最大值</el-col>
        </el-row>
      </el-form-item>
      <el-form-item  label="步数">
        <el-row>
          <el-col :span="12">
            <el-input v-model="dataForm.step.min"></el-input>
          </el-col>
          <el-col :span="12">
            <el-input v-model="dataForm.step.max"></el-input>
          </el-col>
        </el-row>
      </el-form-item>
      <el-form-item  label="心率">
        <el-row>
          <el-col :span="12">
            <el-input v-model="dataForm.heartRate.min"></el-input>
          </el-col>
          <el-col :span="12">
            <el-input v-model="dataForm.heartRate.max"></el-input>
          </el-col>
        </el-row>
      </el-form-item>
      <el-form-item  label="呼吸率">
        <el-row>
          <el-col :span="12">
            <el-input v-model="dataForm.respiratoryRate.min"></el-input>
          </el-col>
          <el-col :span="12">
            <el-input v-model="dataForm.respiratoryRate.max"></el-input>
          </el-col>
        </el-row>
      </el-form-item>
      <el-form-item  label="距离">
        <el-row>
          <el-col :span="12">
            <el-input v-model="dataForm.instance.min"></el-input>
          </el-col>
          <el-col :span="12">
            <el-input v-model="dataForm.instance.max"></el-input>
          </el-col>
        </el-row>
      </el-form-item>
      <el-form-item  label="运动量">
        <el-row>
          <el-col :span="12">
            <el-input v-model="dataForm.sportAmount.min"></el-input>
          </el-col>
          <el-col :span="12">
            <el-input v-model="dataForm.sportAmount.max"></el-input>
          </el-col>
        </el-row>
      </el-form-item>
    </el-form>
    <template slot="footer">
      <el-button @click="visible = false">{{ $t('cancel') }}</el-button>
      <el-button type="primary" @click="dataFormSubmitHandle()">{{ $t('confirm') }}</el-button>
    </template>
  </el-dialog>
</template>

<script>
import debounce from "lodash/debounce";
export default {
  data () {
    return {
      visible: false,
      dataForm: {
        name: "张三",
        telephone: "13012345678",
        step: { min: 100, max: 1000 },
        heartRate: { min: 60, max: 120 },
        respiratoryRate: { min: 10, max: 100 },
        instance: { min: 100, max: 10000 },
        sportAmount: { min: 500, max: 5000 }
      }
    };
  },
  computed: {
    dataRule () {
      return {
        goodsName: [
          {
            required: true,
            message: this.$t("validate.required"),
            trigger: "blur"
          }
        ]
      };
    }
  },
  methods: {
    init () {
      this.visible = true;
      this.$nextTick(() => {
        this.$refs["dataForm"].resetFields();
      });
    },
    // 获取信息
    getInfo () {},
    // 表单提交
    dataFormSubmitHandle: debounce(
      function () {
        this.visible = false
      },
      1000,
      { leading: true, trailing: false }
    )
  }
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

<template>
  <el-card shadow="never" class="aui-card--fill">
    <p>
      <el-button type="success">导出</el-button>
    </p>
    <div class="details-box" id="ecg-details-dome">
      <div class="details-title">
        基本信息
      </div>
      <div class="details-main">
        <details-info />
      </div>
      <div class="details-title">
        心电情况
      </div>
      <div class="details-main">
        <el-form :inline="true" :model="dataForm">
          <el-form-item>
            <el-date-picker
              v-model="daterange"
              type="datetimerange"
              value-format="yyyy-MM-dd HH:mm:ss"
              :range-separator="$t('datePicker.range')"
              start-placeholder="起止时间"
              end-placeholder="结束时间"
            ></el-date-picker>
          </el-form-item>
          <el-form-item>
            <el-button @click="getDataList()">{{ $t("query") }}</el-button>
          </el-form-item>
          <div class="tjx">心电图</div>
          <el-form-item label="分析时段：">
            <el-radio-group v-model="time">
              <el-radio :label="3">24小时</el-radio>
              <el-radio :label="6">1周</el-radio>
              <el-radio :label="9">1个月</el-radio>
            </el-radio-group>
          </el-form-item>
          <div class="human">
            <el-form-item label="心率：">
              <div>60~100次/分</div>
            </el-form-item>
            <el-form-item label="时段：">
              <div>2019-04-05 14:00</div>
            </el-form-item>
          </div>
          <div class="tjx">心率展示</div>
          <el-form-item label="心电变异性分析：">
            <div>T波改变 异常建议进一步进行检查</div>
          </el-form-item>
        </el-form>
      </div>
    </div>
  </el-card>
</template>
<script>
export default {
  components: {
    "details-info": () => import("@/components/details-info"),
  },
  data () {
    return {
      basicInformation: {
        name: "张三",
        sex: "男",
        age: "22",
        stature: "165",
        weight: "70",
        tel: "16601275207",
        site: "北京市海淀区中关村科技大厦502",
      },
      daterange: "",
      time: "",
      dataForm: ""
    };
  },
  watch: {
    daterange (val) {
      this.dataForm.startDate = val[0]
      this.dataForm.endDate = val[1]
    }
  },
  methods: {
    handleDown () {
      this.$htmlToPdf.downloadPDF(document.querySelector('#ecg-details-dome'), this.$route.name)
    }
  },
};
</script>
<style scoped>
.details > h3 {
  text-align: center;
  line-height: 80px;
  margin: 0;
}
.personal {
  display: flex;
  width: 70%;
}
.personal > p {
  flex: 1;
}
.tjx {
  height: 300px;
  display: flex;
  justify-content: center;
  align-items: center;
  margin-bottom: 20px;
  background-color: #ccc;
}
.human {
  width: 36%;
  display: flex;
}
.human > div {
  flex: 1;
}
</style>

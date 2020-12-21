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
        呼吸率情况
      </div>
      <div class="details-main">
        <el-form :inline="true" :model="dataForm" @keyup.enter.native="getDataList()">
          <el-form-item>
            <el-date-picker
              v-model="daterange"
              type="daterange"
              value-format="yyyy-MM-dd"
              :range-separator="$t('datePicker.range')"
              :start-placeholder="$t('datePicker.start')"
              :end-placeholder="$t('datePicker.end')"
            ></el-date-picker>
          </el-form-item>
          <el-form-item>
            <el-button @click="getDataList()">{{ $t("query") }}</el-button>
          </el-form-item>
          <div class="tjx">呼吸波形数据</div>
          <el-form-item label="分析时段：">
            <el-select v-model="time" placeholder="分析时段">
              <el-option label="24小时" value="hour"></el-option>
              <el-option label="1周" value="week"></el-option>
              <el-option label="1个月" value="month"></el-option>
            </el-select>
          </el-form-item>
          <div class="human">
          <el-form-item label="呼吸率：">
            <div>70次/分</div>
          </el-form-item>
          <el-form-item label="时段：">
            <div>2019-04-05 14:00</div>
          </el-form-item>
        </div>
        <div class="tjx">呼吸率展示</div>
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
        site: "北京市海淀区中关村科技大厦502"
      },
      daterange: "",
      time: ""
    };
  },
  watch: {
    daterange (val) {
      this.dataForm.startDate = val[0]
      this.dataForm.endDate = val[1]
    }
  },
  methods: {}
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
.human{
  width: 36%;
  display: flex;
}
.human>div{
  flex: 1;
}
</style>

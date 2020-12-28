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
        步频步幅情况
      </div>
      <div class="details-main">
        <el-form :inline="true" @keyup.enter.native="getDataList()">
          <el-form-item label="分析时段：">
            <el-radio-group v-model="time">
              <el-radio :label="3">24小时</el-radio>
              <el-radio :label="6">1周</el-radio>
              <el-radio :label="9">1个月</el-radio>
            </el-radio-group>
          </el-form-item>
          <div class="human"></div>
          <div class="frequency">
            <div>步频展示</div>
            <div>步幅展示</div>
          </div>
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
    };
  },
  methods: {},
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
}
.human {
  width: 36%;
  display: flex;
}
.human > div {
  flex: 1;
}
.frequency {
  display: flex;
}
.frequency > div {
  flex: 1;
  height: 300px;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: #ccc;
  margin: 0 10px;
}
</style>

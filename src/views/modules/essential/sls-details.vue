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
            <el-radio-group v-model="type">
              <el-radio :label="0">24小时</el-radio>
              <el-radio :label="1">1周</el-radio>
              <el-radio :label="2">1个月</el-radio>
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
  data() {
    return {
      type: 0,
      userId: "",
      dataList: [],
    };
  },
  created() {
    this.userId = this.$route.params.userId;
    console.log(this.$route.params);
    if (this.userId) {
      this.getStepRateAndStride();
    }
  },
  methods: {
    getStepRateAndStride() {
      this.$http
        .get("/step/getStepRateAndStride", {
          params: {
            patientId: this.userId,
          },
        })
        .then(({ data: res }) => {
          if (res.code !== 0) {
            return this.$message.error(res.msg);
          }
          this.dataList = res.data;
        })
        .catch(() => {});
    },
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

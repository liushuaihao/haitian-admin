<template>
  <el-card shadow="never" class="aui-card--fill">
    <p>
      <el-button type="success">导出</el-button>
    </p>
    <div class="details-box">
      <div class="details-title">
        基本信息
      </div>
      <div class="details-main">
        <details-info />
      </div>
      <div class="details-title">
        运动量情况
      </div>
      <div class="details-main">
        <el-form
          :inline="true"
          :model="dataForm"
        >
          <el-form-item label="分析时段：">
            <el-select v-model="time" placeholder="分析时段">
              <el-option label="24小时" value="hour"></el-option>
              <el-option label="1周" value="week"></el-option>
              <el-option label="1个月" value="month"></el-option>
            </el-select>
          </el-form-item>
          <div class="five">
            <el-form-item label="步数：">
              <div>60</div>
            </el-form-item>
            <el-form-item label="距离：">
              <div>100</div>
            </el-form-item>
            <el-form-item label="运动强度：">
              <div>20</div>
            </el-form-item>
            <el-form-item label="消耗热量：">
              <div>80</div>
            </el-form-item>
            <el-form-item label="能量代谢：">
              <div>20</div>
            </el-form-item>
          </div>
          <div class="frequency">
            <div>运动步数</div>
            <div>运动距离</div>
          </div>
          <div class="frequency">
            <div>运动强度曲线</div>
            <div>运动量曲线</div>
          </div>
          <div class="frequency">
            <div>消耗热量</div>
            <div>能量代谢当量</div>
          </div>
          <div class="tjx">综合运动量评估</div>
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
  background-color: #ccc;
}
.human {
  width: 36%;
  display: flex;
}
.human > div {
  flex: 1;
}
.five {
  width: 60%;
  display: flex;
}
.five > div {
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
  margin-bottom: 20px;
}
</style>

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
        定位信息
      </div>
      <div class="details-main">
        <el-form>
          <div class="human">
            <el-form-item label="经度：">
              <div>117.087013</div>
            </el-form-item>
            <el-form-item label="纬度：">
              <div>36.200089</div>
            </el-form-item>
          </div>
        </el-form>
        <gpsMap />
      </div>
    </div>
  </el-card>
</template>
<script>
import gpsMap from "./gps-map.vue";
export default {
  components: {
    gpsMap,
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
      }
    };
  },
  mounted () {
    this.map();
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
#container {
  overflow: hidden;
  width: 100%;
  height: 500px;
  margin: 0;
  font-family: "微软雅黑";
}
.human {
  width: 36%;
  display: flex;
}
.human > div {
  flex: 1;
}
</style>

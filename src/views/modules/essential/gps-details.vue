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
        <el-form :inline="true" v-if="position">
          <el-form-item label="经度：">
            <div>{{ position.baiduLat }}</div>
          </el-form-item>
          <el-form-item label="纬度：">
            <div>{{ position.baiduLng }}</div>
          </el-form-item>
        </el-form>
        <gpsMap :position="position" />
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
  data() {
    return {
      position: null,
      userId: "",
    };
  },
  created() {
    this.userId = this.$route.params.userId;
    if (this.userId) {
      this.getLatestLocationByUserId();
    }
  },
  mounted() {
    this.map();
  },
  methods: {
    getLatestLocationByUserId() {
      this.$http
        .get("/gps/getLatestLocationByUserId", {
          params: {
            id: this.userId,
          },
        })
        .then(({ data: res }) => {
          if (res.code !== 0) {
            return this.$message.error(res.msg);
          }
          this.position = res.data;
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

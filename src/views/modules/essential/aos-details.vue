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
         睡眠分析
      </div>
      <div class="details-main">
        <p>根据基础数据分析睡眠状态，睡眠情况，睡眠质量等，根据基础数据分析睡眠状态，睡眠情况，睡眠质量等根据基础数据分析睡眠状态，睡眠情况，睡眠质量等根据基础数据分析睡眠状态，睡眠情况，睡眠质量等，根据基础数据分析睡眠状态，睡眠情况，睡眠质量等根据基础数据分析睡眠状态，睡眠情况，睡眠质量等
        </p>
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
      }
    };
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
.sleep{
  padding-bottom: 100px;
}
</style>

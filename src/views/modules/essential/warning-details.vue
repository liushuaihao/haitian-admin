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
        预警内容
      </div>
      <div class="details-main">
        <p>
          Maecenas eu elit fringilla, sollicitudin magna eget, convallis tellus.
          Morbi eget elementum magna. Cras lacinia risus id purus malesuada,
          mattis posuere sapien aliquet. Praesent porttitor lobortis
          sollicitudin. Sed dolor nisi, tincidunt a nibh et, malesuada molestie
          sem. Maecenas elementum auctor est. Cras vitae consectetur eros.
          Aliquam sodales elementum pretium. Nulla non lacus a magna iaculis
          vulputate nec eu tortor. Nunc commodo lacinia posuere. Sed eu enim ut
          odio finibus efficitur. Ut sit amet commodo quam, id ullamcorper est.
          Maecenas aliquet ipsum tortor, sit amet facilisis nisi efficitur in.
          Integer congue rhoncus feugiat.
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
        site: "北京市海淀区中关村科技大厦502",
      },
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
.sleep {
  padding-bottom: 100px;
}
</style>

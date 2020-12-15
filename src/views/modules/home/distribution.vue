<template>
  <!-- 高级搜索 -->
  <el-card shadow="never" class="aui-card--fill flex-center">
    <div
      class="mod-home_distribution"
      id="myChartChina"
      :style="{ width: '100%' }"
    ></div>
  </el-card>
</template>

<script>
export default {
  data () {
    return {
      title: "",
      dataList: [],
    };
  },
  // 钩子函数  不了解的话 建议看看 vue的生命周期
  mounted () {
    this.chart();
  },
  methods: {
    chart () {
      //   let that = this;
      // 基于准备好的dom，初始化echarts实例
      var myChartContainer = document.getElementById("myChartChina");
      var resizeMyChartContainer = function () {
        myChartContainer.style.width = document.body.offsetWidth / 1.3 + "px"; // 页面一半的大小
        myChartContainer.style.height = document.body.offsetHeight / 1.3 + "px"; // 页面一半的大小
      };
      resizeMyChartContainer();
      var myChartChina = this.$echarts.init(myChartContainer);
      // 绘制图表
      var optionMap = {
        tooltip: {},
        geo: {
          map: "china",
          itemStyle: {
            // 定义样式
            normal: {
              // 普通状态下的样式
              areaColor: "#323c48",
              borderColor: "#111",
            },
            emphasis: {
              // 高亮状态下的样式
              areaColor: "#2a333d",
            },
          },
        },
        color: "#31B531",
        legend: {
          orient: "vertical",
          left: "left",
        },
        selectedMode: "single",
        series: [
          {
            type: "map",
            mapType: "china",
            data: [],
          },
        ],
      };

      myChartChina.setOption(optionMap);
      window.addEventListener("resize", function () {
        resizeMyChartContainer();
        myChartChina.resize();
      });
    },
  },
  beforeDestroy () {
    window.removeEventListener("resize", this.chart);
  },
};
</script>

<style lang="scss" scoped>
.flex-center {
  display: flex;
  align-items: center;
  justify-content: center;
  .el-card__body {
    display: flex !important;
    align-items: center !important;
    justify-content: center !important;
  }
}
</style>

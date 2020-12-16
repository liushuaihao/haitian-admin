<template>
  <!-- 高级搜索 -->
  <el-card shadow="never"  class="aui-card--fill flex-center">
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
      isShow: false,
      myChartChina: null
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
        myChartContainer.style.height = " calc(calc(100vh - 50px - 38px - 70px) - 2px)"; // 页面一半的大小
      };
      resizeMyChartContainer();
      var myChartChina = this.$echarts.init(myChartContainer);
      this.myChartChina = myChartChina
      // 绘制图表
      var optionMap = {
        tooltip: {},
        geo: {
          map: "china",
          itemStyle: {
            // 定义样式
            normal: {
              // 普通状态下的样式
              areaColor: '#0064B3',
              borderColor: '#0080B4',
            },
            emphasis: {
              // 高亮状态下的样式
              areaColor: '#83C9E9',
            }
          },
        },
        legend: {
          orient: "vertical",
          left: "left",
        },
        selectedMode: "single",
        visualMap: {
          min: 0,
          x: "right",
          y: "bottom",
          right: "10%",
          calculable: true
        },
        series: [
          {
            name: '销量',
            type: 'scatter',
            coordinateSystem: 'geo',
            data: [
              {name: '海门', value: [121.15, 31.89, 90]},
              {name: '鄂尔多斯', value: [109.781327, 39.608266, 120]},
              {name: '招远', value: [120.38, 37.35, 142]},
              {name: '舟山', value: [122.207216, 29.985295, 123]},
            ] // series数据内容
          },
          {
            name: "全部数据",
            type: "map",
            mapType: "china",
            itemStyle: {
              normal: {
                borderColor: "rgba(0, 0, 0, 0.2)"
              },
              emphasis: {
                shadowOffsetX: 0,
                shadowOffsetY: 0,
                shadowBlur: 20,
                borderWidth: 0,
                shadowColor: "rgba(0, 0, 0, 0.5)"
              }
            },
            showLegendSymbol: false,
            label: {
              normal: {
                show: false
              },
              emphasis: {
                show: false
              }
            },
            data: []
          }
        ]
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
  }
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

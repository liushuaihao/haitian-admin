<template>
  <div class="mod-home_distribution" id="myChartChina"></div>
</template>

<script>
export default {
  data() {
    return {
      title: "",
      dataList: [],
      isShow: true,
      myChartChina: null,
    };
  },
  // 钩子函数  不了解的话 建议看看 vue的生命周期
  mounted() {
    this.chart();
  },
  methods: {
    chart() {
      var data = [
        { name: "北京", value: 177 },
        { name: "天津", value: 42 },
        { name: "河北", value: 102 },
        { name: "山西", value: 81 },
        { name: "内蒙古", value: 47 },
        { name: "辽宁", value: 67 },
        { name: "吉林", value: 82 },
        { name: "黑龙江", value: 66 },
        { name: "上海", value: 24 },
        { name: "江苏", value: 92 },
        { name: "浙江", value: 114 },
        { name: "安徽", value: 109 },
        { name: "福建", value: 116 },
        { name: "江西", value: 91 },
        { name: "山东", value: 119 },
        { name: "河南", value: 137 },
        { name: "湖北", value: 116 },
        { name: "湖南", value: 114 },
        { name: "重庆", value: 91 },
        { name: "四川", value: 125 },
        { name: "贵州", value: 62 },
        { name: "云南", value: 83 },
        { name: "西藏", value: 9 },
        { name: "陕西", value: 80 },
        { name: "甘肃", value: 56 },
        { name: "青海", value: 10 },
        { name: "宁夏", value: 18 },
        { name: "新疆", value: 67 },
        { name: "广东", value: 123 },
        { name: "广西", value: 59 },
        { name: "海南", value: 14 },
        { name: "台湾", value: 14 },
        { name: "南海诸岛", value: 14 },
      ];

      var mapName = "china";
      // 基于准备好的dom，初始化echarts实例
      var myChartContainer = document.getElementById("myChartChina");
      // var resizeMyChartContainer = function() {
      //   myChartContainer.style.width = document.body.offsetWidth / 1.5 + "px"; // 页面一半的大小
      //   myChartContainer.style.height = `80vh`; // 页面一半的大小
      // };
      // resizeMyChartContainer();
      var myChartChina = this.$echarts.init(myChartContainer);
      this.myChartChina = myChartChina;
      // 绘制图表
      var optionMap = {
        tooltip: {},
        visualMap: {
          type: "piecewise",
          pieces: [
            {
              min: 100,
              max: 200,
              label: "100-200人",
              color: "red",
            },
            { min: 0, max: 100, label: "0-100人", color: "green" },
          ],
        },
        silent: false,
        geo: {
          show: true,
          map: mapName,
          label: {
            normal: {
              show: false,
            },
            emphasis: {
              show: false,
            },
          },
          itemStyle: {
            normal: {
              //地图背景色
              areaColor: "#eee",
            },
            emphasis: {
              //鼠标放上去区域背景色
              areaColor: "#eee",
            },
          },
          // roam: true,
          tooltip: {
            formatter: function(params, ticket, callback) {
              console.log();
              let html = params.marker;
              return html + params.name + "：" + params.value[2] + "人";
            },
          },
        },
        legend: {
          orient: "vertical",
          left: "center",
          bottom: "10px",
        },
        selectedMode: "single",
        series: [
          {
            type: "scatter",
            coordinateSystem: "geo",
            data: [
              { name: "河南", value: [113.65, 34.76, 3] },
              { name: "新疆", value: [87.62, 43.82, 5] },
              { name: "四川", value: [104.07, 30.1, 109] },
              { name: "西藏", value: [91.13, 29.1, 104] },
            ],
          },
        ],
      };

      myChartChina.setOption(optionMap);
      window.addEventListener("resize", function() {
        // resizeMyChartContainer();
        myChartChina.resize();
      });
    },
  },
  beforeDestroy() {
    window.removeEventListener("resize", this.chart);
  },
  activated() {
    this.isShow = true;
  },
  deactivated() {
    this.isShow = false;
  },
};
</script>

<style lang="scss" scoped>
.mod-home_distribution {
  position: absolute;
  left: 0;
  right: 0;
  top: 0;
  bottom: 0;
}
</style>

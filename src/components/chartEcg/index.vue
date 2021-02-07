<template>
  <div class="chart1">
    <chartTemp :option="option"></chartTemp>
  </div>
</template>

<script>
import chartTemp from "@/components/chartTemp";
export default {
  props: {
    dataList: {
      type: Array,
      default: () => {
        return [];
      },
    },
  },
  components: {
    chartTemp,
  },
  data() {
    return {
      option: {},
      dateList: [],
      valueList: [],
    };
  },
  mounted() {
    this.getEchartsData();
  },
  watch: {
    dataList: {
      handler: function(newV) {
        this.dateList = newV.map(function(item) {
          return item.countTime;
        });
        this.valueList = newV.map(function(item) {
          return item.heartRate;
        });
        this.getEchartsData();
        console.log(this.dateList, this.valueList);
      },
      deep: true,
    },
  },
  methods: {
    getEchartsData() {
      this.option = {
        tooltip: {
          trigger: "axis",
        },
        xAxis: {
          data: this.dateList,
        },
        yAxis: {
          type: "value",
        },
        series: [
          {
            type: "line",
            showSymbol: false,
            hoverAnimation: false,
            data: this.valueList,
          },
        ],
      };
    },
  },
};
</script>

<style lang="scss" scoped>
.chart1 {
  height: 400px;
}
</style>

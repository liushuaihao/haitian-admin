<template>
  <!-- 公共图表组件
    author:heyongkui
    功能：echarts图表公共组件
    三个参数：icon  title   option
    icon 为标题图标         非必须传
    title 为标题            非必须传
    options为echarts需要的option     必须传 不传显示为空

    注：因此组件为公共组ss件 请不要擅自改动此组件 如本组件不满足需求 请联系大佬修改
   -->
  <div class="chart-container" ref="container">
    <!-- <div
      class="chart-title"
      v-if="title != ''"
      ref="title"
      :style="{ 'padding-left': icon ? '30px' : '0px' }"
    >
      
       <i v-if="icon!=''" :class="[icon]"></i> 
      <img v-if="icon != ''" :src="icon" alt class="chart-icon-img" />
      <span class="chart-title-text" v-html="title"></span>
    </div> -->
    <div
      class="chart-body"
      ref="chartDiv"
      :id="echartsId"
      :style="layout"
      v-if="ispage"
    ></div>
    <!-- -->
  </div>
</template>

<script>
// 引入防抖函数
import { debounce } from "@/utils";
export default {
  props: {
    title: {
      type: String,
      default: "",
    },
    icon: {
      type: String,
      default: "",
    },
    option: {
      type: Object,
      default() {
        return null;
      },
    },
    layout: {
      type: Object,
      default() {
        return {};
      },
    },
  },
  data() {
    return {
      chartObj: null,
      chartData: {},
      chartDom: null,
      Dom: null,
      resizeHanlder: null,
      ispage: true,
    };
  },
  mounted() {
    this.chartDom = document.getElementById(this.echartsId);
    this.Dom = document.querySelector(".el-tabs__content");
    console.log(this.Dom);
    this.chartObj = this.$echarts.init(this.chartDom);
    this.drawChart();
    this.resizeHanlder = debounce(this.refreshChart);
    // 添加尺寸改变事件
    this.Dom.addEventListener("resize", this.resizeHanlder);
    window.addEventListener("resize", this.resizeHanlder);
  },
  watch: {
    sidebarFold: {
      immediate: true,
      handler: function(newval) {
        console.log(newval);
        debounce(this.refreshChart);
      },
    },
    option() {
      this.drawChart();
    },
  },
  computed: {
    echartsId() {
      return "echarts" + Math.random() * 100000;
    },
    sidebarFold() {
      return this.$store.state.sidebarFold;
    },
  },
  methods: {
    // 刷新图标
    refresh() {
      this.drawChart();
    },
    clearChart() {
      this.chartDom.innerHtml = "";
    },
    resizeChart() {
      let titleHeight = this.$refs["title"].clientHeight;
      let totleHeight = this.$refs["container"].clientHeight;
      this.style = {
        height: totleHeight - titleHeight + "px",
      };
      this.refreshChart();
    },
    refreshChart() {
      console.log("this.chartObj", this.chartObj.resize);
      this.chartObj.resize();
    },
    drawChart() {
      this.clearChart();
      if (this.option && Object.keys(this.option).length > 0) {
        this.chartObj.setOption(this.option);
      }
    },
    setOption(option) {
      this.chartObj.setOption(option);
      this.drawChart();
    },
    setProps(props) {
      if (Object.keys(props).length > 0) {
        for (let key in props) {
          this[key] = props[key];
        }
        this.$nextTick(() => {
          this.resizeChart();
          this.drawChart();
        });
      }
    },
  },
  beforeDestroy() {
    this.Dom.removeEventListener("resize", this.refreshChart);
    window.removeEventListener("resize", this.refreshChart);
  },
  components: {},
};
</script>
<style scoped lang="scss">
.chart-container {
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
}

.chart-title {
  height: 40px;
  line-height: 40px;
  position: relative;
}

.chart-title-text {
  display: inline-block;
  padding: 0px 15px;
  font-size: 16px;
}

.chart-body {
  flex: 1;
}

.chart-icon-img {
  width: 33px;
  height: 33px;
  position: absolute;
  left: 10px;
  top: 3px;
}
</style>

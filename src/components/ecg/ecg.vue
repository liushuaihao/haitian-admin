<template>
  <div>
    <div class="sssss">
      <canvas :id="grids" width="780" height="150"></canvas>
      <div class="sss">
        <canvas
          style="border:1px solid #ff3232;"
          width="780"
          height="150"
          :id="myCanvas"
        ></canvas>
      </div>
    </div>
    <!-- <button @click="startOrStop">{{ ifStart ? "暂停" : "开始" }}</button> -->
  </div>
</template>

<script>
export default {
  name: "lookEcgs",
  props: {
    //props用来接收，一定是放在data()的外面，不要放在data()里面。
    arr: {
      //title一定是在父组件绑定的值。父组件绑定什么，子组件接收的这就写什么
      type: Array, // type:是接受过来的类型。一定要是什么类型在子组件里定义
      default: () => {
        return []; //没有return会报错，而null 需求而定 也可以空数组等[]
      },
    },
    grids: {
      //title一定是在父组件绑定的值。父组件绑定什么，子组件接收的这就写什么
      type: String, // type:是接受过来的类型。一定要是什么类型在子组件里定义
      default: () => {
        return null; //没有return会报错，而null 需求而定 也可以空数组等[]
      },
    },
    ifStarts: {
      //title一定是在父组件绑定的值。父组件绑定什么，子组件接收的这就写什么
      type: Boolean, // type:是接受过来的类型。一定要是什么类型在子组件里定义
      default: () => {
        return; //没有return会报错，而null 需求而定 也可以空数组等[]
      },
    },
    myCanvas: {
      //title一定是在父组件绑定的值。父组件绑定什么，子组件接收的这就写什么
      type: String, // type:是接受过来的类型。一定要是什么类型在子组件里定义
      default: () => {
        return null; //没有return会报错，而null 需求而定 也可以空数组等[]
      },
    },
  },
  data() {
    return {
      canvasTxt: "",
      // arr : [],
      startLoop: false,
      ifStart: this.ifStarts,
      myTimeout: null,
      line: 0,
      xNum: 0.27,
      yNum: 0.065,
      width: 800,
      height: 150,
    };
  },
  watch: {
    ifStarts(val) {
      if (val) {
        this.loop();
      } else {
        clearTimeout(this.myTimeout);
      }
    },
  },
  methods: {
    dian() {
      let canvas = document.getElementById(this.myCanvas);
      this.canvasTxt = canvas.getContext("2d");
      this.loop();
    },
    // 画网格
    drawgrid() {
      var c_canvas = document.getElementById(this.grids);
      c_canvas.getContext("2d").clearRect(0, 0, this.width, this.height);
      this.drawSmallGrid(c_canvas);
      this.drawMediumGrid(c_canvas);
      this.drawBigGrid(c_canvas);
    },
    getArr(arr) {
      if (arr.length >= parseInt((2 * this.width) / this.xNum + 1)) {
        return arr.slice(0, parseInt((2 * this.width) / this.xNum + 1));
      } else {
        return this.getArr(
          arr.concat(
            arr.slice(
              0,
              parseInt((2 * this.width) / this.xNum + 1) - arr.length
            )
          )
        );
      }
    },
    loop() {
      this.canvasTxt.clearRect(0, 0, this.width, this.height);
      let arr = this.getArr(this.arr);
      this.canvasTxt.beginPath();
      if (this.line === -1 * parseInt(this.arr.length / this.xNum))
        this.line = 0;
      this.drawgrid();
      this.canvasTxt.moveTo(
        this.line,
        parseInt(this.height / 2) - arr[0] * this.yNum
      );
      for (let i = 0; i < arr.length - 1; i++) {
        let endY = parseInt(this.height / 2) - arr[i + 1] * this.yNum;
        this.canvasTxt.lineTo(this.line + (i + 1) * this.xNum, endY);
      }
      this.canvasTxt.stroke();
      this.line -= 1;
      // this.myTimeout = setTimeout(this.loop, 60);
    },
    //小网格
    drawSmallGrid(canvas) {
      let ctx = canvas.getContext("2d");
      ctx.strokeStyle = "#f1dedf";
      ctx.lineWidth = 1;
      ctx.beginPath();
      for (var x = 0.5; x < 780 - this.line; x += 1) {
        ctx.moveTo(x + this.line, 0);
        ctx.lineTo(x + this.line, 780);
      }
      ctx.stroke();
      return;
    },
    //中网格
    drawMediumGrid(canvas) {
      let ctx = canvas.getContext("2d");
      ctx.strokeStyle = "#ccc";
      ctx.lineWidth = 1;
      //宽度是小网格的2倍
      ctx.beginPath();
      for (var x = 0.5; x < 780 - this.line; x += 4.89) {
        ctx.moveTo(x + this.line, 0);
        ctx.lineTo(x + this.line, 780);
      }
      ctx.stroke();
      for (var y = 0.5; y < 780; y += 4.89) {
        ctx.moveTo(0, y);
        ctx.lineTo(780, y);
      }
      ctx.stroke();
      // ctx.closePath();
      return;
    },
    //大网格
    drawBigGrid(canvas) {
      let ctx = canvas.getContext("2d");
      ctx.strokeStyle = "#e0514b";
      ctx.lineWidth = 1;
      ctx.beginPath();
      for (var x = 0.5; x < 780 - this.line; x += 24.45) {
        ctx.moveTo(x + this.line, 0);
        ctx.lineTo(x + this.line, 780);
      }
      ctx.stroke();
      for (var y = 0.5; y < 780; y += 24.5) {
        ctx.moveTo(0, y);
        ctx.lineTo(780, y);
      }
      ctx.stroke();
      return;
    },

    jinzhi(lsit) {
      let list = lsit.split(",");
      //    console.log(list)
      let yu1 = ["7fff"];
      let yu2 = ["8000"];
      let yu1_v = parseInt(yu1, 16);
      let yu2_v = parseInt(yu2, 16);
      let list_xin_2 = [];
      list.forEach((item) => {
        item = item.replace("0X", ""); //16进制 比如 0x0006   拿的其实就是后四位。这是这一步的来历
        let yuan = parseInt(item, 16);
        let ins = yuan & yu2_v;
        if (ins == 0) {
          // console.log(String(yuan))
          list_xin_2.push(String(yuan));
        } else {
          let intsss = yuan & yu1_v;
          let fu_item = "-" + intsss;
          list_xin_2.push(fu_item);
        }
      });
      return list_xin_2;
      console.log(list_xin_1);
    },
  },
  destroyed() {
    clearTimeout(this.myTimeout);
  },
  mounted() {
    this.$nextTick(() => this.dian());
  },
};
</script>

<style scoped>
.sssss {
  position: relative;
}
.sss {
  position: absolute;
  top: 0px;
  left: 0px;
}
</style>

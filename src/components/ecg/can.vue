<template>
  <div>
    <div class="sssss">
      <canvas :id="grids" width="751px" height="751px"></canvas>
      <div class="sss">
        <canvas
          style="border:1px solid #ff3232;"
          width="751px"
          height="751px"
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
      // this.loop();
      this.drawgrid();
    },
    // 画网格
    drawgrid() {
      var c_canvas = document.getElementById(this.grids);
      c_canvas.getContext("2d").clearRect(0, 0, this.width, this.height);
      this.drawSmallGrid(c_canvas);
      this.drawMediumGrid(c_canvas);
      this.drawBigGrid(c_canvas);
      this.drawLine(c_canvas);
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
      var context = canvas.getContext("2d");
      context.strokeStyle = "#f1dedf";
      context.strokeWidth = 1;
      context.beginPath();
      for (var x = 0.5; x < 751; x += 3) {
        context.moveTo(x, 0);
        context.lineTo(x, 751);
        context.stroke();
      }

      for (var y = 0.5; y < 751; y += 3) {
        context.moveTo(0, y);
        context.lineTo(751, y);
        context.stroke();
      }
      context.closePath();
      return;
    },
    //中网格
    drawMediumGrid(canvas) {
      var context = canvas.getContext("2d");
      context.strokeStyle = "#f0adaa";
      context.strokeWidth = 1;
      context.beginPath();
      for (var x = 0.5; x < 751; x += 15) {
        context.moveTo(x, 0);
        context.lineTo(x, 751);
        context.stroke();
      }

      for (var y = 0.5; y < 751; y += 15) {
        context.moveTo(0, y);
        context.lineTo(751, y);

        context.stroke();
      }
      context.closePath();
      return;
    },
    //大网格
    drawBigGrid(canvas) {
      var context = canvas.getContext("2d");
      context.strokeStyle = "#e0514b";
      context.strokeWidth = 1;
      context.beginPath();
      for (var x = 0.5; x < 751; x += 75) {
        context.moveTo(x, 0);
        context.lineTo(x, 751);
        context.stroke();
      }

      for (var y = 0.5; y < 751; y += 75) {
        context.moveTo(0, y);
        context.lineTo(751, y);
        context.stroke();
      }
      context.closePath();
      return;
    },
    // 线条
    drawLine(canvas) {
      var ctx = canvas.getContext("2d");
      ctx.strokeStyle = "#000";
      ctx.strokeWidth = 1;
      ctx.beginPath();
      // ctx.moveTo(0.5, 200);
      // ctx.lineTo(50, 200);

      let arr = [
        1927,
        1953,
        1951,
        1969,
        2001,
        2375,
        3263,
        2660,
        1600,
        1796,
        1915,
        1926,
        1935,
        1991,
        1981,
        2000,
        1994,
        2021,
        2038,
        2080,
        2131,
        2184,
        2225,
        2289,
        2351,
        2401,
        2418,
        2409,
        2317,
        2271,
        2126,
        2016,
        1911,
        1874,
        1819,
        1832,
        1814,
        1820,
        1836,
        1876,
        1859,
        1866,
        1866,
        1863,
        1881,
        1936,
        1940,
        1948,
        1973,
        2053,
        2021,
        2018,
        2059,
        2050,
        1989,
        2017,
        2105,
        2169,
        2125,
        2115,
        2077,
        2065,
        2071,
        2045,
        2041,
        2031,
        2042,
        2019,
        2073,
        2745,
        3422,
        2049,
        1538,
        1821,
        1935,
        1931,
        1937,
        1966,
        1967,
        2011,
        1983,
        1980,
        2004,
        2040,
        2053,
        2096,
        2157,
        2243,
        2290,
        2353,
        2363,
        2319,
        2219,
        2208,
        2077,
        1971,
        1852,
        1783,
        1759,
        1774,
        1757,
        1782,
        1801,
        1807,
        1854,
        1879,
        1871,
        1885,
        1882,
        1908,
        1863,
        1831,
        1849,
        1841,
        1817,
        1862,
        1869,
        1858,
        1879,
        1937,
        1924,
        1920,
        1882,
        1903,
        1911,
        1903,
        1893,
        1922,
        1906,
        1909,
        1911,
        2274,
        3245,
        2817,
        1583,
        1733,
        1870,
        1900,
        1907,
        1939,
        1946,
        1998,
        2033,
        2064,
        2051,
        2095,
        2150,
        2195,
        2235,
        2293,
        2368,
        2413,
        2436,
        2413,
        2356,
        2270,
        2119,
        2031,
        1937,
        1915,
        1865,
        1854,
        1844,
        1849,
        1865,
        1900,
        1885,
        1913,
        1900,
        1927,
        1930,
        1953,
        1942,
        1966,
        1978,
        1992,
        1951,
        1980,
        1962,
        1937,
        1919,
        1912,
        1990,
        2024,
        2005,
        1926,
        1899,
        1945,
        1923,
        1931,
        1922,
        1926,
        1921,
        1916,
        2018,
        2719,
        3370,
        2002,
        1613,
        1833,
        1874,
        1889,
        1889,
        1926,
        1927,
        1925,
        1949,
        1963,
        1998,
        2046,
        2064,
        2109,
        2154,
        2226,
        2290,
        2342,
        2350,
        2313,
        2237,
        2166,
        2060,
        1965,
        1893,
        1836,
        1815,
        1833,
        1834,
        1841,
        1846,
        1899,
        1895,
        1932,
        1940,
        1936,
        1912,
        1941,
        1908,
        1914,
        1886,
        1905,
        1914,
        1907,
        1895,
        1923,
        1889,
        1902,
        1944,
        1910,
        1967,
        2039,
        1999,
        1988,
        1950,
        1943,
        1920,
        1935,
        1920,
        1952,
        1960,
        1923,
        1943,
        2506,
        3345,
        2274,
        1546,
        1763,
        1835,
        1856,
        1871,
        1896,
        1891,
        1937,
        1981,
        1996,
        1993,
        2041,
        2097,
        2139,
        2182,
        2287,
        2336,
        2410,
        2428,
        2397,
        2324,
        2259,
        2128,
        2015,
        1889,
        1828,
        1792,
        1797,
        1765,
        1776,
        1760,
        1795,
        1802,
        1804,
        1859,
        1866,
        1858,
        1866,
        1877,
        1913,
        1912,
        1925,
        1913,
        1914,
        1924,
        1929,
        1963,
        1954,
        1931,
        1952,
        1928,
        1975,
        2053,
        2077,
        2026,
        1959,
        1947,
        1938,
        1948,
        1943,
        1988,
        2032,
        1986,
        2053,
        2638,
        3417,
        2188,
        1618,
        1827,
        1938,
        1932,
        1941,
        1953,
        2009,
        1982,
        2010,
        2018,
        2066,
        2091,
        2105,
        2143,
        2204,
        2302,
        2391,
        2398,
        2437,
        2387,
        2353,
        2292,
        2155,
        2030,
        1940,
        1854,
        1871,
        1858,
        1845,
        1833,
        1830,
        1861,
        1884,
        1874,
        1895,
        1893,
        1942,
        1914,
        1934,
        1931,
        1943,
        1909,
        1906,
        1905,
        1925,
        1906,
        1916,
        1917,
        1936,
        1980,
        2042,
        2037,
        2055,
        2026,
        2017,
        2016,
        2002,
        1982,
        1985,
        1960,
        1954,
        2006,
        2483,
        3405,
        2511,
        1541,
        1750,
        1891,
        1923,
        1939,
        1972,
        1997,
        2027,
        2074,
        2107,
        2127,
        2132,
        2149,
        2201,
        2230,
        2292,
        2376,
        2441,
        2421,
        2381,
        2356,
        2285,
        2165,
        2069,
        1967,
        1885,
        1831,
        1822,
        1802,
        1816,
        1844,
        1883,
        1896,
        1915,
        1908,
        1923,
        1931,
        1941,
        1949,
        2016,
        2010,
        1991,
        1957,
        1971,
        1965,
        1977,
        1966,
        1947,
        1925,
        2016,
        2060,
        2045,
        1988,
        1961,
        1963,
        1983,
        1971,
        2011,
        2021,
        2050,
        2035,
        2182,
        2875,
        3394,
        1969,
        1695,
        1895,
        1948,
        1937,
        1939,
        1988,
        2031,
        2010,
        2009,
        2029,
        2061,
        2096,
        2134,
        2135,
        2219,
        2294,
        2359,
        2393,
        2419,
        2349,
        2293,
        2180,
        2059,
        1963,
        1884,
        1811,
        1779,
        1752,
        1748,
        1769,
        1791,
        1803,
        1832,
        1819,
        1834,
        1851,
        1887,
        1907,
        1925,
      ];
      arr.map((item, index) => {
        ctx.lineTo(index + 1, item / 10);
        console.log(index + 1, item);
      });
      // for (var x = 9; x < 117; x++) {
      //   ctx.lineTo(x * 6, (Math.random() * 10 - 5) * 20 + 200);
      //   console.log(x * 6, (Math.random() * 10 - 5) * 20 + 200);
      // }

      // ctx.lineTo(700, 200);
      // ctx.lineTo(750, 200);
      ctx.stroke();
      ctx.closePath();
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

    // //*************************************使用示例****************************
    //   let luo2_zhuan =this.jinzhi(luo2_number)
    //   this.arr_v11 = luo2_zhuan.map(Number)  //数字是字符串的必须转number类型，要不然canvas不认。
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

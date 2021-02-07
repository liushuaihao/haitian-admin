<template>
  <el-card shadow="never" class="aui-card--fill">
    <p>
      <el-button type="success" v-if="$hasPermission('business:report:edit')"
        >修改</el-button
      >
      <el-button type="success" v-if="$hasPermission('business:report:expor')"
        >导出</el-button
      >
    </p>
    <div class="details-box">
      <div class="details-title">
        基本信息
      </div>
      <div class="details-main">
        <details-info />
      </div>
      <!-- 心电情况 -->
      <div class="details-title">
        心电情况
      </div>
      <div class="details-main">
        <el-form label-width="140px" label="">
          <el-form-item label="心律图：">
            <el-form-item label="数据区间选择：">
              <el-select v-model="breatheDate" placeholder="请选择数据区间">
                <el-option label="9：00~9：40" value="shanghai"></el-option>
                <el-option label="11：00~12：00" value="beijing"></el-option>
              </el-select>
            </el-form-item>
          </el-form-item>
          <el-form-item label="24小时心律图：">
            <div class="tjx">24小时心率图</div>
          </el-form-item>
          <el-form-item label="医生诊断：">
            <el-input v-model="val1"></el-input>
          </el-form-item>
        </el-form>
      </div>
      <!-- 呼吸情况 -->
      <div class="details-title">
        呼吸情况
      </div>
      <div class="details-main">
        <el-form label-width="140px" label="">
          <el-form-item label="呼吸波形图：">
            <el-form-item label="数据区间选择：">
              <el-select v-model="breatheDate" placeholder="请选择数据区间">
                <el-option label="9：00~9：40" value="shanghai"></el-option>
                <el-option label="11：00~12：00" value="beijing"></el-option>
              </el-select>
            </el-form-item>
          </el-form-item>
          <el-form-item label="24小时呼吸率图：">
            <div class="tjx">呼吸波形图</div>
            <div class="tjx">24小时呼吸律图</div>
          </el-form-item>
          <el-form-item label="医生诊断：">
            <el-input v-model="val3"></el-input>
          </el-form-item>
        </el-form>
      </div>
      <!-- 运动量 -->
      <div class="details-title">
        运动量
      </div>
      <div class="details-main">
        <el-form label-width="140px" label="">
          <el-form-item label="24小时运动数据：">
            <el-row :gutter="20">
              <el-col :span="12"><div class="tjx">运动步数</div></el-col>
              <el-col :span="12"><div class="tjx">运动距离</div></el-col>
            </el-row>
            <el-row :gutter="20">
              <el-col :span="12"><div class="tjx">运动强度曲线</div></el-col>
              <el-col :span="12"><div class="tjx">运动量曲线</div></el-col>
            </el-row>
            <el-row :gutter="20">
              <el-col :span="12"><div class="tjx">消耗热量</div></el-col>
              <el-col :span="12"><div class="tjx">能量代谢当量</div></el-col>
            </el-row>
            <el-row :gutter="20">
              <el-col :span="12"><div class="tjx">综合运动量评估</div></el-col>
            </el-row>
          </el-form-item>
          <el-form-item label="医生诊断：">
            <el-input v-model="val4"></el-input>
          </el-form-item>
        </el-form>
      </div>
      <!-- 运动状态 -->
      <div class="details-title">
        运动状态
      </div>
      <div class="details-main">
        <el-form label-width="140px" label="">
          <el-form-item label="运动波形图：">
            <el-form-item label="数据区间选择：">
              <el-select v-model="breatheDate" placeholder="请选择数据区间">
                <el-option label="9：00~9：40" value="shanghai"></el-option>
                <el-option label="11：00~12：00" value="beijing"></el-option>
              </el-select>
            </el-form-item>
          </el-form-item>
          <el-form-item label="24小时运动状态：">
            <el-row :gutter="20">
              <el-col :span="12"><div class="tjx">机电波形数据图</div></el-col>
              <el-col :span="12"><div class="tjx">加速度波形图</div></el-col>
            </el-row>
            <el-row :gutter="20">
              <el-col :span="12"><div class="tjx">四肢运动轨迹图</div></el-col>
              <el-col :span="12"
                ><div class="tjx">肌力数值波形数据</div></el-col
              >
            </el-row>
            <el-row :gutter="20">
              <el-col :span="12"
                ><div class="tjx">四肢运动数据曲线展示</div></el-col
              >
            </el-row>
          </el-form-item>
          <el-form-item label="医生诊断：">
            <el-input v-model="val5"></el-input>
          </el-form-item>
        </el-form>
      </div>
      <!-- 预警信息 -->
      <div class="details-title">
        预警信息
      </div>
      <div class="details-main">
        <el-form label-width="140px" label="">
          <el-form-item label="24小时异常报警：">
            <div>心电异常</div>
            <div>步数异常</div>
          </el-form-item>
          <el-form-item label="医生诊断：">
            <el-input v-model="val2"></el-input>
          </el-form-item>
        </el-form>
      </div>
      <!-- 睡眠状态 -->
      <div class="details-title">
        睡眠状态
      </div>
      <div class="details-main">
        <el-form label-width="140px" label="">
          <el-form-item label="睡眠状态：">
            <div>
              睡眠状态睡眠状态睡眠状态睡眠状态睡眠状态睡眠状态睡眠状态睡眠状态
            </div>
          </el-form-item>
          <el-form-item label="医生诊断：">
            <el-input v-model="val6"></el-input>
          </el-form-item>
        </el-form>
      </div>
    </div>
  </el-card>
</template>
<script>
export default {
  components: {
    "details-info": () => import("@/components/details-info"),
  },
  data() {
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
      breatheDate: "",
      ecgDate: "",
      dialogFormVisible: false,
      val1: "",
      val2: "",
      val3: "",
      val4: "",
      val5: "",
      val6: "",
    };
  },
  methods: {},
};
</script>
<style lang="scss" scoped>
.details-main {
  .el-row {
    margin-left: 0 !important;
    margin-right: 0 !important;
    .tjx {
      height: 80px;
    }
  }
}

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
.tjx {
  height: 150px;
  margin-bottom: 20px;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: #ccc;
}
.human {
  width: 36%;
  display: flex;
}
.human > div {
  flex: 1;
}
.three {
  width: 50%;
  display: flex;
}
.three > div {
  flex: 1;
}
</style>

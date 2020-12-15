<template>
  <el-card shadow="never" class="aui-card--fill">
    <el-form ref="form" :model="form" label-width="100px">
      <el-form-item label="商品名称：">
        <div>{{type|mold}}</div>
      </el-form-item>
      <el-form-item label="单价：" v-if="type==1">
        <div>3个月/600.00；6个月/900.00；12个月/1500.00</div>
      </el-form-item>
      <el-form-item label="单价：" v-if="type==2">
        <div>上肢/500.00；下肢/500.00；</div>
        <div>心电/500.00；呼吸/500.00；</div>
        <div>肌电/500.00</div>
      </el-form-item>
      <el-form-item label="单价：" v-if="type==3">
        <div>上肢/500.00，押金500.00；</div>
        <div>上肢/500.00，押金500.00；</div>
        <div>上肢/500.00，押金500.00；</div>
        <div>上肢/500.00，押金500.00；</div>
        <div>上肢/500.00，押金500.00；</div>
      </el-form-item>
      <el-form-item label="说明：" v-if="type==1">
        <div>在线服务提供健康报告诊断、医生即时问诊服务。</div>
      </el-form-item>
      <el-form-item label="说明：" v-if="type==2">
        <div>上肢运动检测模块可长时间穿戴的采集手腕的运动数据；</div>
        <div>下肢运动检测模块可长时间穿戴的采集手腕的运动数据；</div>
        <div>心电检测模块可长时间穿戴的采集心电数据；</div>
        <div>肌电检测模块可长时间穿戴的测量肌电数据；</div>
        <div>呼吸检测模块可长时间穿戴的测量呼吸数据</div>
      </el-form-item>
      <el-form-item label="说明：" v-if="type==3">
        <div>上肢运动检测模块可长时间穿戴的采集手腕的运动数据；</div>
        <div>下肢运动检测模块可长时间穿戴的采集手腕的运动数据；</div>
        <div>心电检测模块可长时间穿戴的采集心电数据；</div>
        <div>肌电检测模块可长时间穿戴的测量肌电数据；</div>
        <div>呼吸检测模块可长时间穿戴的测量呼吸数据</div>
      </el-form-item>
      <el-form-item label="服务分类：" v-if="type==1">
        <el-tag
          v-for="item in taItems"
          :key="item.label"
          :type="item.type"
          effect="dark"
          class="tag"
        >{{ item.label }}</el-tag>
      </el-form-item>
      <el-form-item label="选择品种：" v-if="type==2">
        <el-select placeholder="选择品种" v-model="variety">
          <el-option label="上肢运动检测模块" value="shang"></el-option>
          <el-option label="下肢运动检测模块" value="xia"></el-option>
        </el-select>
      </el-form-item>
      <el-form-item label="设备分类：" v-if="type==3">
        <el-select placeholder="设备分类：" v-model="variety">
          <el-option label="上肢运动检测模块" value="shang"></el-option>
          <el-option label="下肢运动检测模块" value="xia"></el-option>
        </el-select>
      </el-form-item>
      <el-form-item label="租凭时长：" v-if="type==3">
        <el-select placeholder="租凭时长：" v-model="variety2">
          <el-option label="1个月" value="one"></el-option>
          <el-option label="2个月" value="tow"></el-option>
        </el-select>
      </el-form-item>
    </el-form>
    <el-button type="primary" @click="retu">返回</el-button>
  </el-card>
</template>

<script>
export default {
  data () {
    return {
      type: this.$route.params.type,
      variety: "", // 品种
      variety2: "",
      taItems: [
        { type: "", label: "3个月" },
        { type: "", label: "6个月" },
        { type: "", label: "12个月" }
      ]
    };
  },
  methods: {
    retu: function () {
      this.$router.go(-1);
    }
  },
  filters: {
    mold (type) {
      if (type === 1) {
        return "在线服务";
      } else if (type === 2) {
        return "设备购买";
      } else if (type === 3) {
        return "设备租赁";
      }
    }
  }
};
</script>
<style scoped>
.tag {
  margin-right: 20px !important;
}
</style>

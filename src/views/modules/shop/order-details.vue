<template>
  <el-card shadow="never" class="aui-card--fill">
    <el-form ref="form" :model="form" label-width="100px">
      <el-form-item label="订单编号：">
        <div>{{particularsData.serial}}</div>
      </el-form-item>
      <el-form-item label="发货时间：">
        <div>{{particularsData.deliverDate}}</div>
      </el-form-item>
      <el-form-item label="支付方式：">
        <div>{{particularsData.pay}}</div>
      </el-form-item>
      <el-form-item label="交易流水号：">
        <div>{{particularsData.running}}</div>
      </el-form-item>
      <el-form-item label="商品总价：">
        <div>{{particularsData.total}}</div>
      </el-form-item>
      <el-form-item label="购买者ID：">
        <div>{{particularsData.id}}</div>
      </el-form-item>
      <el-form-item label="购买者姓名：">
        <div>{{particularsData.name}}</div>
      </el-form-item>
      <el-form-item label="产品有效期：">
        <div>{{particularsData.period}}</div>
      </el-form-item>
      <el-form-item label="订单状态：">
        <div>{{particularsData.state}}</div>
      </el-form-item>
    </el-form>
    <el-table
      v-loading="dataListLoading"
      :data="dataList"
      border
      @selection-change="dataListSelectionChangeHandle"
      @sort-change="dataListSortChangeHandle"
      style="width: 100%;"
    >
      <el-table-column prop="name" label="名称" header-align="center" align="center"></el-table-column>
      <el-table-column prop="num" label="数量" header-align="center" align="center"></el-table-column>
      <el-table-column prop="price" label="单价(元)" header-align="center" align="center"></el-table-column>
    </el-table>
    <el-button type="primary" @click="retu" class="btn">返回</el-button>
  </el-card>
</template>

<script>
export default {
  data () {
    return {
      particularsData: {
        serial: "1101110111",
        deliverDate: "2019-05-08 8:00",
        pay: "支付宝",
        running: "11011101114552114552222",
        total: "600.00",
        id: "1233221",
        name: "王明",
        period: "无",
        state: "已完成"
      },
      dataList: [
        { name: "中控设备", num: "1", price: "100.00" },
        { name: "上肢运动模块", num: "1", price: "200.00" },
        { name: "心电监测模块", num: "1", price: "300.00" }
      ]
    };
  },
  methods: {
    retu: function () {
      this.$router.go(-1);
    }
  }
};
</script>
<style scoped>
.btn{
    margin-top: 20px !important;
}
</style>

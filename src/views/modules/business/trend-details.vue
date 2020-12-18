<template>
  <el-card>
    <el-form :inline="true" :model="dataForm" @keyup.enter.native="getDataList()">
      <el-form-item>
        <el-date-picker
          v-model="daterange"
          type="daterange"
          value-format="yyyy-MM-dd"
          :range-separator="$t('datePicker.range')"
          :start-placeholder="$t('datePicker.start')"
          :end-placeholder="$t('datePicker.end')"
        ></el-date-picker>
      </el-form-item>
      <el-form-item>
        <el-button @click="getDataList()">{{ $t("query") }}</el-button>
      </el-form-item>
      <el-form-item label="统计项" class="tjx-ledt">
        <el-select v-model="dataForm.System" placeholder="统计项">
          <el-option label="全部测试项目" value="all"></el-option>
          <el-option label="心电" value="xindian"></el-option>
          <el-option label="呼吸" value="huxi"></el-option>
          <el-option label="步频步幅" value="bubinbufu"></el-option>
          <el-option label="运动及步态" value="yundong"></el-option>
          <el-option label="运动量" value="yundongliang"></el-option>
        </el-select>
      </el-form-item>
    </el-form>

    <div v-if="dataForm.System=='xindian'||dataForm.System=='all'" class="tjx">
      <h4>心电情况统计</h4>
    </div>

    <div v-if="dataForm.System=='huxi'||dataForm.System=='all'" class="tjx">
      <h4>呼吸情况统计</h4>
    </div>

    <div v-if="dataForm.System=='bubinbufu'||dataForm.System=='all'" class="tjx">
      <h4>步频步幅度情况统计</h4>
    </div>

    <div v-if="dataForm.System=='yundong'||dataForm.System=='all'" class="tjx">
      <h4>运动及步态情况统计</h4>
    </div>

    <div v-if="dataForm.System=='yundongliang'||dataForm.System=='all'" class="tjx">
      <h4>运动量情况统计</h4>
    </div>
  </el-card>
</template>
<script>
import mixinViewModule from "@/mixins/view-module";
import { addDynamicRoute } from "@/router"; // 添加动态路由
export default {
  mixins: [mixinViewModule],
  data () {
    return {
      mixinViewModuleOptions: {
        getDataListURL: "",
        getDataListIsPage: true,
        deleteURL: "",
        deleteIsBatch: true
      },
      dataForm: {
        identityCard: "",
        name: "",
        mobile: "",
        id: "",
        System: "all"
      },
      daterange: null,
      dataList: [
        {
          id: "1111-0000-1111",
          name: "张三",
          mobile: "13012345671",
          identityCard: "11011011123456789"
        },
        {
          id: "1111-0000-1111",
          name: "张三",
          mobile: "13012345671",
          identityCard: "11011011123456789"
        },
        {
          id: "1111-0000-1111",
          name: "张三",
          mobile: "13012345671",
          identityCard: "11011011123456789"
        },
        {
          id: "1111-0000-1111",
          name: "张三",
          mobile: "13012345671",
          identityCard: "11011011123456789"
        },
        {
          id: "1111-0000-1111",
          name: "张三",
          mobile: "13012345671",
          identityCard: "11011011123456789",
          state: "完成",
          tmti: "2019-04-05"
        }
      ]
    };
  },
  methods: {
    forwardUrl (row) {
      var routeParams = {
        routeName: `${this.$route.name}__instance_${row.id}`,
        menuId: `${this.$route.meta.menuId}`,
        title: `${this.$route.meta.title}详情 - ${row.name}`,
        path: "business/statistic-details",
        params: {}
      };
      addDynamicRoute(routeParams, this.$router, this.$route);
    }
  }
};
</script>
<style scoped>
    .tjx{
        height: 360px;
        display: flex;
        justify-content:center;
        align-items:center;
        background-color: #ccc;
        margin-bottom: 20px;
    }
    .tjx-ledt{
      margin-left: 50px;
    }
</style>

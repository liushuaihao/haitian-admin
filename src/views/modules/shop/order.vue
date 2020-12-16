<template>
  <el-card shadow="never" class="aui-card--fill">
    <el-form :inline="true" :model="dataForm" @keyup.enter.native="getDataList()">
      <el-form-item>
        <el-input v-model="dataForm.identityCard" placeholder="订单号" clearable></el-input>
      </el-form-item>
      <el-form-item>
        <el-input v-model="dataForm.name" placeholder="身份证" clearable></el-input>
      </el-form-item>
      <el-form-item>
        <el-input v-model="dataForm.mobile" placeholder="姓名" clearable></el-input>
      </el-form-item>
      <el-form-item>
        <el-input v-model="dataForm.id" placeholder="手机号" clearable></el-input>
      </el-form-item>
      <el-form-item>
        <el-select class="selects" clearable v-model="condition" placeholder="订单状态">
          <el-option v-for="item in states" :key="item.value" :label="item.label" :value="item.value"></el-option>
        </el-select>
      </el-form-item>
      <el-form-item>
        <el-button @click="getDataList()">{{ $t('query') }}</el-button>
      </el-form-item>
      <el-form-item>
        <el-button type="danger" @click="deleteHandle()">{{ $t('deleteBatch') }}</el-button>
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
      <el-table-column prop="order" label="订单号" header-align="center" align="center"></el-table-column>
      <el-table-column prop="name" label="姓名" header-align="center" align="center"></el-table-column>
      <el-table-column prop="mobile" label="电话" header-align="center" align="center"></el-table-column>
      <el-table-column prop="status" label="订单状态" header-align="center" align="center"></el-table-column>
      <el-table-column
        prop="tmti"
        label="操作时间"
        sortable="custom"
        header-align="center"
        align="center"
      >
        <!-- <template slot-scope="scope">
            <el-tag v-if="scope.row.status === 1" size="small">{{ $t('mail.status1') }}</el-tag>
            <el-tag v-else size="small" type="danger">{{ $t('mail.status0') }}</el-tag>
        </template>-->
      </el-table-column>
      <!-- <el-table-column
          prop="createDate"
          :label="$t('mail.createDate')"
          sortable="custom"
          header-align="center"
          align="center"
          width="180"
      ></el-table-column>-->
      <el-table-column
        :label="$t('handle')"
        fixed="right"
        header-align="center"
        align="center"
        width="150"
      >
        <template slot-scope="scope">
          <el-button type="text" size="small" @click="forwardUrl(scope.row)">查看</el-button>
        </template>
      </el-table-column>
    </el-table>
    <div class="mod-essential_aoe">
      <el-pagination
        :current-page="page"
        :page-sizes="[10, 20, 50, 100]"
        :page-size="limit"
        :total="total"
        layout="total, sizes, prev, pager, next, jumper"
        @size-change="pageSizeChangeHandle"
        @current-change="pageCurrentChangeHandle"
      ></el-pagination>
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
      condition: '',
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
        id: ""
      },
      dataList: [
        {
          order: "00001",
          name: "张三",
          mobile: "13012345671",
          status: "已下单",
          heart: "60", // 心率值
          tmti: "2019-04-05" // 最近时间
        },
        {
          order: "00002",
          name: "张三",
          mobile: "13012345671",
          status: "已下单",
          heart: "70",
          tmti: "2019-04-06"
        },
        {
          order: "00003",
          name: "张三",
          mobile: "13012345671",
          status: "已付款",
          heart: "60",
          tmti: "2019-04-07"
        },
        {
          order: "00004",
          name: "张三",
          mobile: "13012345671",
          status: "已下单",
          heart: "80",
          tmti: "2019-04-08"
        },
        {
          order: "00005",
          name: "张三",
          mobile: "13012345671",
          status: "已下单",
          heart: "50",
          tmti: "2019-04-09"
        }
      ],
      states: [
        {
          value: "选项1",
          label: "全部"
        },
        {
          value: "选项2",
          label: "已下单"
        },
        {
          value: "选项3",
          label: "已付款"
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
        path: "shop/order-details",
        params: {}
      };
      addDynamicRoute(routeParams, this.$router, this.$route);
    }
  }
};
</script>
<style scoped>
.selects{
  margin-bottom:20px !important;
}
</style>

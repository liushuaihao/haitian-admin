<template>
  <el-card shadow="never" class="aui-card--fill">
    <el-form :inline="true" @keyup.enter.native="getDataList()" class="order">
      <el-form-item label="订单编号">
        <el-input v-model="dataForm.serial" placeholder="订单编号" clearable></el-input>
      </el-form-item>
      <el-form-item label="用户ID">
        <el-input v-model="dataForm.id" placeholder="用户ID" clearable></el-input>
      </el-form-item>
      <el-form-item label="姓名">
        <el-input v-model="dataForm.name" placeholder="姓名" clearable></el-input>
      </el-form-item>
      <el-form-item label="手机号">
        <el-input v-model="dataForm.mobile" placeholder="手机号" clearable></el-input>
      </el-form-item>
      <el-form-item label="身份证">
        <el-input v-model="dataForm.identityCard" placeholder="身份证" clearable></el-input>
      </el-form-item>
      <el-form-item>
        <el-button >筛选</el-button>
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
      <el-table-column prop="status" label="状态" header-align="center" align="center">
        <template slot-scope="scope">
          <div>
            <el-tag v-if="scope.row.status === '已下单'">{{scope.row.status}}</el-tag>
            <el-tag v-else>{{scope.row.status}}</el-tag>
          </div>
        </template>
      </el-table-column>
      <el-table-column
        prop="tmti"
        label="下单时间"
        sortable="custom"
        header-align="center"
        align="center"
      >
        <!-- <template slot-scope="scope">
            <el-tag v-if="scope.row.status === 1" size="small">{{ $t('mail.status1') }}</el-tag>
            <el-tag v-else size="small" type="danger">{{ $t('mail.status0') }}</el-tag>
        </template>-->
      </el-table-column>
      <el-table-column
        prop="tmti"
        label="支付时间"
        sortable="custom"
        header-align="center"
        align="center"
      >
      </el-table-column>
      <el-table-column
        label="支付方式"
        header-align="center"
        align="center"
        width="150"
      >
        <template slot-scope="scope">
          <div v-if="scope.row.status === '已付款'">
            {{'微信支付'}}
          </div>
        </template>
      </el-table-column>
      <el-table-column
        :label="$t('handle')"
        header-align="center"
        align="center"
        width="150"
      >
        <template slot-scope="scope">
          <el-button style="color:#E6A23C" type="text" size="small" @click="sipping(scope.row)" v-if="scope.row.status === '已付款'">
           发货
          </el-button>
          <el-button type="text" size="small" @click="forwardUrl(scope.row)">详情</el-button>
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
      mixinViewModuleOptions: {
        getDataListURL: "",
        getDataListIsPage: true,
        deleteURL: "",
        deleteIsBatch: true
      },
      dataForm: {
        serial: "",
        id: "",
        name: "",
        mobile: "",
        identityCard: ""
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
    // 发货
    sipping (id) {
      this.$confirm('订单发货', '发货', {
        confirmButtonText: this.$t('confirm'),
        cancelButtonText: this.$t('cancel'),
        type: 'warning'
      }).then(() => {
      }).catch(() => {})
    },
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
.order>div{
margin-right: 30px;
}
</style>

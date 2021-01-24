<template>
  <el-card shadow="never" class="aui-card--fill">
    <el-form :inline="true" @keyup.enter.native="getDataList()">
      <el-form-item>
        <el-input
          v-model="dataForm.orderId"
          placeholder="订单编号"
          clearable
        ></el-input>
      </el-form-item>
      <el-form-item>
        <el-input
          v-model="dataForm.userId"
          placeholder="用户ID"
          clearable
        ></el-input>
      </el-form-item>
      <el-form-item>
        <el-input
          v-model="dataForm.realName"
          placeholder="姓名"
          clearable
        ></el-input>
      </el-form-item>
      <el-form-item>
        <el-input
          v-model="dataForm.mobile"
          placeholder="手机号"
          clearable
        ></el-input>
      </el-form-item>
      <el-form-item>
        <el-input
          v-model="dataForm.idCard"
          placeholder="身份证"
          clearable
        ></el-input>
      </el-form-item>
      <el-form-item>
        <el-select
          v-model="dataForm.orderStatus"
          style="width:250px"
          placeholder="订单状态"
          clearable
        >
          <el-option
            v-for="(key, item, index) in orderStatus"
            :key="index"
            :label="key"
            :value="item"
          ></el-option>
        </el-select>
      </el-form-item>
      <el-form-item>
        <el-date-picker
          v-model="daterange"
          type="datetimerange"
          value-format="yyyy-MM-dd HH:mm:ss"
          :range-separator="$t('datePicker.range')"
          start-placeholder="起止时间"
          end-placeholder="结束时间"
        >
        </el-date-picker>
      </el-form-item>
      <el-form-item>
        <el-button @click="getDataList()">{{ $t("query") }}</el-button>
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
      <el-table-column
        prop="id"
        label="订单号"
        header-align="center"
        align="center"
      ></el-table-column>
      <el-table-column
        prop="realName"
        label="姓名"
        header-align="center"
        align="center"
      ></el-table-column>
      <el-table-column
        prop="mobile"
        label="手机号"
        header-align="center"
        align="center"
      ></el-table-column>
      <el-table-column
        prop="orderStatus"
        label="状态"
        header-align="center"
        align="center"
      >
        <template slot-scope="scope">
          <div>
            <!-- 0已下单、1已付款、2已完成、3已取消 -->
            <el-tag v-if="scope.row.orderStatus == 0">已下单</el-tag>
            <el-tag v-if="scope.row.orderStatus == 1">已付款</el-tag>
            <el-tag v-if="scope.row.orderStatus == 2" type="success"
              >已完成</el-tag
            >
            <el-tag v-if="scope.row.orderStatus == 3" type="danger"
              >已取消</el-tag
            >
          </div>
        </template>
      </el-table-column>
      <el-table-column
        prop="createDate"
        label="下单时间"
        header-align="center"
        align="center"
      >
      </el-table-column>
      <el-table-column
        prop="payTime"
        label="支付时间"
        header-align="center"
        align="center"
      >
      </el-table-column>
      <el-table-column
        label="支付方式"
        prop="payChannel"
        header-align="center"
        align="center"
        width="150"
      >
        <template slot-scope="scope">
          <!--  0微信 1支付宝 -->
          <el-tag v-if="scope.row.payChannel == 0" type="success">微信</el-tag>
          <el-tag v-if="scope.row.payChannel == 1">支付宝</el-tag>
        </template>
      </el-table-column>
      <el-table-column
        :label="$t('handle')"
        fixed="right"
        header-align="center"
        align="center"
        width="150"
      >
        <template slot-scope="scope">
          <el-button
            style="color:#E6A23C"
            type="text"
            size="small"
            @click="updateTransStatus(scope.row.id)"
            v-if="
              scope.row.orderStatus == 1 && $hasPermission('shop:order:update')
            "
          >
            发货
          </el-button>
          <el-button
            type="text"
            size="small"
            @click="forwardUrl(scope.row)"
            v-if="$hasPermission('shop:order:info')"
            >详情</el-button
          >
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
  data() {
    return {
      mixinViewModuleOptions: {
        getDataListURL: "/shop/order/list",
        getDataListIsPage: true,
        activatedIsNeed: true,
        deleteURL: "",
        deleteIsBatch: true,
      },
      daterange: null,
      orderStatus: {
        0: "已下单",
        1: "已付款",
        2: "已完成",
        3: "已取消",
      },
    };
  },

  watch: {
    daterange(val) {
      if (val) {
        this.dataForm.beginTime = val[0];
        this.dataForm.endTime = val[1];
      } else {
        this.dataForm.beginTime = undefined;
        this.dataForm.endTime = undefined;
      }
    },
  },
  methods: {
    // 发货
    updateTransStatus(id) {
      this.$confirm("订单发货", "发货", {
        confirmButtonText: this.$t("confirm"),
        cancelButtonText: this.$t("cancel"),
        type: "warning",
      })
        .then(() => {
          this.$http
            .post("/shop/order/updateTransStatus", {
              id: id,
            })
            .then(({ data: res }) => {
              if (res.code !== 0) {
                return this.$message.error(res.msg);
              }

              this.$message({
                message: "发货成功",
                type: "success",
                duration: 500,
                onClose: () => {
                  this.getDataList();
                },
              });
            })
            .catch(() => {});
        })
        .catch(() => {});
    },
    forwardUrl(row) {
      var routeParams = {
        routeName: `${this.$route.name}__instance_${row.id}`,
        menuId: `${this.$route.meta.menuId}`,
        title: `${this.$route.meta.title}详情`,
        path: "shop/order-details",
        params: {
          id: row.id,
        },
      };
      addDynamicRoute(routeParams, this.$router, this.$route);
    },
  },
};
</script>
<style scoped>
.selects {
  margin-bottom: 20px !important;
}
.order > div {
  margin-right: 30px;
}
</style>

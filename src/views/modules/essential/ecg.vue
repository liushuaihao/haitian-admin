<template>
  <!-- 心电 -->
  <el-card shadow="never" class="aui-card--fill">
    <div class="mod-essential_ecg">
      <el-form
        :inline="true"
        :model="dataForm"
        @keyup.enter.native="getDataList()"
      >
        <el-form-item>
          <el-date-picker
            v-model="daterange"
            type="datetimerange"
            value-format="yyyy-MM-dd HH:mm:ss"
            :range-separator="$t('datePicker.range')"
            start-placeholder="起止时间"
            end-placeholder="结束时间"
          ></el-date-picker>
        </el-form-item>
        <el-form-item>
          <el-input
            v-model="dataForm.idCard"
            :placeholder="$t('essential.identityCard')"
            clearable
          ></el-input>
        </el-form-item>
        <el-form-item>
          <el-input
            v-model="dataForm.realName"
            :placeholder="$t('essential.name')"
            clearable
          ></el-input>
        </el-form-item>
        <el-form-item>
          <el-input
            v-model="dataForm.mobile"
            :placeholder="$t('essential.mobile')"
            clearable
          ></el-input>
        </el-form-item>
        <el-form-item>
          <el-input
            v-model="dataForm.deviceId"
            :placeholder="$t('essential.id')"
            clearable
          ></el-input>
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
          prop="realName"
          :label="$t('essential.name')"
          header-align="center"
          align="center"
        ></el-table-column>
        <el-table-column
          prop="mobile"
          :label="$t('essential.mobile')"
          header-align="center"
          align="center"
        ></el-table-column>
        <el-table-column
          prop="idCard"
          :label="$t('essential.identityCard')"
          header-align="center"
          align="center"
        ></el-table-column>
        <el-table-column
          prop="deviceId"
          :label="$t('essential.id')"
          header-align="center"
          align="center"
        ></el-table-column>
        <el-table-column
          prop="heartRate"
          label="心率"
          header-align="center"
          align="center"
        ></el-table-column>
        <el-table-column
          prop="beginTime"
          label="最近时间"
          sortable="custom"
          header-align="center"
          align="center"
        >
          <!-- <template slot-scope="scope">
            <el-tag>{{ scope.row.beginTime }}</el-tag>
            <el-tag>{{ scope.row.endTime }}</el-tag>
          </template> -->
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
            <el-button type="text" size="small" @click="forwardUrl(scope.row)"
              >查看</el-button
            >
          </template>
        </el-table-column>
      </el-table>
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
        // getDataListURL: "/ecg/list",
        getDataListIsPage: true,
        deleteURL: "",
        deleteIsBatch: true,
      },
      daterange: null,
      dataList: [{}],
    };
  },
  watch: {
    daterange(val) {
      if (val) {
        this.dataForm.beginTime = val[0];
        this.dataForm.endTime = val[1];
      } else {
        this.getNowFormatDate();
      }
    },
  },
  methods: {
    getNowFormatDate() {
      var date = new Date();
      var seperator1 = "-";
      var year = date.getFullYear();
      var month = date.getMonth() + 1;
      var strDate = date.getDate();
      if (month >= 1 && month <= 9) {
        month = "0" + month;
      }
      if (strDate >= 0 && strDate <= 9) {
        strDate = "0" + strDate;
      }
      var currentdate = year + seperator1 + month + seperator1 + strDate;

      this.dataForm.beginTime = currentdate + "00:00:00";
      this.dataForm.endTime = currentdate + "23:59:59";
    },
    forwardUrl(row) {
      console.log(row);
      var routeParams = {
        routeName: `${this.$route.name}__instance_${row.userId}`,
        menuId: `${this.$route.meta.menuId}`,
        title: `${this.$route.meta.title}详情 - ${row.realName}`,
        path: "essential/ecg-details",
        params: {
          userId: row.userId,
        },
      };
      addDynamicRoute(routeParams, this.$router, this.$route);
    },
  },
};
</script>

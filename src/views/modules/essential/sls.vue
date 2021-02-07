<template>
  <el-card shadow="never" class="aui-card--fill">
    <div class="mod-mod-essential_sls">
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
          prop="frequency"
          label="步频"
          header-align="center"
          align="center"
        ></el-table-column>
        <el-table-column
          prop="stride"
          label="步幅"
          header-align="center"
          align="center"
        ></el-table-column>
        <el-table-column
          prop="countTime"
          label="最近时间"
          sortable="custom"
          header-align="center"
          align="center"
        >
        </el-table-column>
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
        getDataListURL: "/step/list",
        getDataListIsPage: true,
        deleteURL: "",
        deleteIsBatch: true,
      },
      daterange: null,
    };
  },
  watch: {
    daterange(val) {
      if (val) {
        this.dataForm.beginTime = val[0];
        this.dataForm.endTime = val[1];
      } else {
        this.dataForm.beginTime = null;
        this.dataForm.endTime = null;
      }
    },
  },
  methods: {
    forwardUrl(row) {
      var routeParams = {
        routeName: `${this.$route.name}__instance_${row.userId}`,
        menuId: `${this.$route.meta.menuId}`,
        title: `${this.$route.meta.title}详情 - ${row.realName}`,
        path: "essential/sls-details",
        params: {
          userId: row.userId,
        },
      };
      addDynamicRoute(routeParams, this.$router, this.$route);
    },
  },
};
</script>

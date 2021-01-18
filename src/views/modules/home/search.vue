<template>
  <!-- 高级搜索 -->
  <el-card shadow="never" class="aui-card--fill">
    <div class="mod-home_search">
      <el-form
        :inline="true"
        :model="dataForm"
        @keyup.enter.native="getDataList()"
      >
        <el-form-item>
          <el-input
            v-model="dataForm.mobile"
            :placeholder="$t('user.mobile')"
            clearable
          ></el-input>
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
          prop="realName"
          label="姓名"
          header-align="center"
          align="center"
        ></el-table-column>
        <el-table-column
          prop="age"
          label="年龄"
          header-align="center"
          align="center"
        ></el-table-column>
        <el-table-column
          prop="gender"
          :label="$t('user.gender')"
          header-align="center"
          align="center"
        >
          <template slot-scope="scope">{{
            $getDictLabel("gender", scope.row.gender)
          }}</template>
        </el-table-column>
        <el-table-column
          prop="mobile"
          :label="$t('user.mobile')"
          min-width="110px"
          header-align="center"
          align="center"
        ></el-table-column>
        <el-table-column
          prop="idCard"
          label="身份证号"
          min-width="200px"
          header-align="center"
          align="center"
        ></el-table-column>

        <el-table-column
          prop="userType"
          label="角色"
          align="center"
          width="80"
        ></el-table-column>
        <el-table-column
          prop="cityName"
          label="所属区域"
          header-align="center"
          min-width="140px"
          align="center"
        ></el-table-column>
        <el-table-column
          prop="createDate"
          :label="$t('user.createDate')"
          sortable="custom"
          header-align="center"
          align="center"
          width="180"
        ></el-table-column>
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
        getDataListURL: "/sys/user/search",
        getDataListIsPage: true,
      },
      daterange: null,
      dataForm: {
        mobile: "",
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
    forwardUrl(row) {
      var routeParams = {
        routeName: `${this.$route.name}__instance_${row.id}`,
        menuId: `${this.$route.meta.menuId}`,
        title: `${this.$route.meta.title}详情 - ${row.name}`,
        path: "essential/aoe-details",
        params: {},
      };
      addDynamicRoute(routeParams, this.$router, this.$route);
    },
  },
};
</script>

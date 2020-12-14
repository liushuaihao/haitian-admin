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
            type="daterange"
            value-format="yyyy-MM-dd"
            :range-separator="$t('datePicker.range')"
            :start-placeholder="$t('datePicker.start')"
            :end-placeholder="$t('datePicker.end')"
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
          prop="name"
          :label="$t('updatePassword.username')"
          header-align="center"
          align="center"
        ></el-table-column>
        <el-table-column
          prop="name"
          :label="$t('essential.name')"
          header-align="center"
          align="center"
        ></el-table-column>

        <el-table-column
          prop="mobile"
          label="年龄"
          header-align="center"
          align="center"
        ></el-table-column>
        <el-table-column
          prop="identityCard"
          :label="$t('user.gender')"
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
          prop="identityCard"
          :label="$t('essential.identityCard')"
          header-align="center"
          align="center"
        ></el-table-column>
        <el-table-column
          prop="id"
          label="角色"
          header-align="center"
          align="center"
        ></el-table-column>
        <el-table-column
          prop="heart"
          label="所属区域"
          header-align="center"
          align="center"
        ></el-table-column>
        <el-table-column
          prop="tmti"
          label="添加时间"
          header-align="center"
          align="center"
        >
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
  data () {
    return {
      mixinViewModuleOptions: {
        getDataListURL: "",
        getDataListIsPage: true,
        deleteURL: "",
        deleteIsBatch: true,
      },
      daterange: null,
      dataForm: {
        mobile: "",
      },
      dataList: [
        {
          id: "1111-0000-1111",
          name: "张三",
          mobile: "13012345671",
          identityCard: "11011011123456789",
          heart: "60", // 心率值
          tmti: "2019-04-05", // 最近时间
        },
        {
          id: "1111-0000-1111",
          name: "张三",
          mobile: "13012345671",
          identityCard: "11011011123456789",
          heart: "70",
          tmti: "2019-04-05",
        },
        {
          id: "1111-0000-1111",
          name: "张三",
          mobile: "13012345671",
          identityCard: "11011011123456789",
          heart: "60",
          tmti: "2019-04-05",
        },
        {
          id: "1111-0000-1111",
          name: "张三",
          mobile: "13012345671",
          identityCard: "11011011123456789",
          heart: "80",
          tmti: "2019-04-05",
        },
        {
          id: "1111-0000-1111",
          name: "张三",
          mobile: "13012345671",
          identityCard: "11011011123456789",
          heart: "50",
          tmti: "2019-04-05",
        },
      ],
    };
  },
  watch: {
    daterange (val) {
      this.dataForm.startDate = val[0];
      this.dataForm.endDate = val[1];
    },
  },
  methods: {
    forwardUrl (row) {
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

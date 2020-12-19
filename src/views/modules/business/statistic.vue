<template>
  <!-- 数据统计 -->
  <el-card shadow="never" class="aui-card--fill">
    <div class="mod-mod-essential_sls">
      <el-form :inline="true" :model="dataForm" @keyup.enter.native="getDataList()">
        <el-form-item>
          <el-input
            v-model="dataForm.identityCard"
            :placeholder="$t('essential.identityCard')"
            clearable
          ></el-input>
        </el-form-item>
        <el-form-item>
          <el-input v-model="dataForm.name" :placeholder="$t('essential.name')" clearable></el-input>
        </el-form-item>
        <el-form-item>
          <el-input v-model="dataForm.mobile" :placeholder="$t('essential.mobile')" clearable></el-input>
        </el-form-item>
        <el-form-item>
          <el-input v-model="dataForm.id" :placeholder="$t('essential.id')" clearable></el-input>
        </el-form-item>
        <el-form-item>
          <el-button>{{ $t('query') }}</el-button>
        </el-form-item>
        <el-form-item>
          <el-button type="danger" @click="deleteHandle()">{{ $t('deleteBatch') }}</el-button>
        </el-form-item>
      </el-form>
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
            <el-option label="运动及步态" value="yudong"></el-option>
            <el-option label="运动量" value="yudongliang"></el-option>
          </el-select>
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
          prop="identityCard"
          :label="$t('essential.identityCard')"
          header-align="center"
          align="center"
        ></el-table-column>
        <el-table-column prop="id" :label="$t('essential.id')" header-align="center" align="center"></el-table-column>
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
        identityCard: "",
        name: "",
        mobile: "",
        id: "",
        System: ""
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
.tjx-ledt {
  margin-left: 50px;
}
</style>

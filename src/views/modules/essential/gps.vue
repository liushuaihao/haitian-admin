<template>
  <el-card shadow="never" class="aui-card--fill">
    <div class="mod-mod-essential_gps">
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
          <el-button @click="getDataList()">{{ $t('query') }}</el-button>
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
        <el-table-column prop="twt" :label="$t('essential.type')" header-align="center" align="center"></el-table-column>
        <el-table-column
          prop="tmti"
          label="预警时间"
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
      },
      dataList: [
        {
          name: "张三",
          mobile: "13012345671",
          identityCard: "11011011123456789",
          twt: "心电异常", // 预警类型
          tmti: "2019-04-05" // 预警时间
        },
        {
          name: "张三",
          mobile: "13012345671",
          identityCard: "11011011123456789",
          twt: "步态异常",
          tmti: "2019-04-05"
        },
        {
          name: "张三",
          mobile: "13012345671",
          identityCard: "11011011123456789",
          twt: "呼吸异常",
          tmti: "2019-04-05"
        },
        {
          name: "张三",
          mobile: "13012345671",
          identityCard: "11011011123456789",
          twt: "运动异常",
          tmti: "2019-04-05"
        },
        {
          name: "张三",
          mobile: "13012345671",
          identityCard: "11011011123456789",
          twt: "运动异常",
          tmti: "2019-04-05"
        },
      ]
    };
  },
  methods: {
    forwardUrl (row) {
      var routeParams = {
        routeName: `${this.$route.name}__instance_${row.id}`,
        menuId: `${this.$route.meta.menuId}`,
        title: `${this.$route.meta.title}详情 - ${row.name}`,
        path: "essential/gps-details",
        params: {}
      };
      addDynamicRoute(routeParams, this.$router, this.$route);
    }
  }
};
</script>

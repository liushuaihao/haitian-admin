<template>
  <el-card shadow="never" class="aui-card--fill">
    <div class="mod-sys__dept">
      <el-form
        :inline="true"
        :model="dataForm"
        @keyup.enter.native="getDataList()"
      >
        <!-- 机构名称/所属地区 -->
        <el-form-item>
          <el-input
            placeholder="机构名称"
            v-model="dataForm.orgName"
          ></el-input>
        </el-form-item>
        <el-form-item>
          <el-input placeholder="所属地区" v-model="dataForm.cityId"></el-input>
        </el-form-item>
        <el-form-item>
          <el-button @click="getDataList()">{{ $t("query") }}</el-button>
          <el-button type="primary" @click="addOrUpdateHandle()">{{
            $t("add")
          }}</el-button>
        </el-form-item>
      </el-form>
      <el-table
        v-loading="dataListLoading"
        :data="dataList"
        row-key="id"
        border
        style="width: 100%;"
      >
        <el-table-column
          prop="orgName"
          label="机构名称"
          header-align="center"
        ></el-table-column>
        <el-table-column
          prop="cityId"
          label="所属地区"
          header-align="center"
          align="center"
        ></el-table-column>
        <el-table-column
          prop="telephone"
          label="电话"
          header-align="center"
          align="center"
        ></el-table-column>
        <el-table-column
          prop="createDate"
          label="添加时间"
          header-align="center"
          align="center"
        ></el-table-column>
        <el-table-column
          :label="$t('handle')"
          fixed="right"
          header-align="center"
          align="center"
          width="200"
        >
          <template slot-scope="scope">
            <el-button
              type="text"
              size="small"
              @click="addOrUpdateHandle(scope.row.id)"
              >{{ $t("update") }}</el-button
            >
            <el-button
              type="text"
              size="small"
              @click="deleteHandle(scope.row.id)"
              >{{ $t("delete") }}</el-button
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

      <!-- 弹窗, 新增 / 修改 -->
      <add-or-update
        v-if="addOrUpdateVisible"
        ref="addOrUpdate"
        @refreshDataList="getDataList"
      ></add-or-update>
    </div>
  </el-card>
</template>

<script>
import mixinViewModule from "@/mixins/view-module";
import AddOrUpdate from "./index-add-or-update";
export default {
  mixins: [mixinViewModule],
  data() {
    return {
      mixinViewModuleOptions: {
        getDataListURL: "/sys/org/page",
        getDataListIsPage: true,
        deleteURL: "/sys/org/delete",
        deleteIsBatch: true,
      },
      dataList: [],
    };
  },
  components: {
    AddOrUpdate,
  },
};
</script>

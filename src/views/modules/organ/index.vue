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
          <el-cascader
            style="width:100%;"
            v-model="dataForm.cityId"
            placeholder="选择区域"
            :options="regionTree"
            :props="optionsProps"
            @change="handleChange"
            clearable
          ></el-cascader>
        </el-form-item>
        <el-form-item>
          <el-button @click="getDataList()">{{ $t("query") }}</el-button>
          <el-button
            v-if="$hasPermission('dept:org:save')"
            type="primary"
            @click="addOrUpdateHandle()"
            >{{ $t("add") }}</el-button
          >
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
          prop="fullName"
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
              v-if="$hasPermission('dept:org:update')"
              @click="addOrUpdateHandle(scope.row.id)"
              >{{ $t("update") }}</el-button
            >
            <el-button
              type="text"
              size="small"
              v-if="$hasPermission('dept:org:delete')"
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
        :regionTree="regionTree"
        :optionsProps="optionsProps"
      ></add-or-update>
    </div>
  </el-card>
</template>

<script>
import mixinViewModule from "@/mixins/view-module";
import AddOrUpdate from "./index-add-or-update";
import { treeDataTranslate } from "@/utils";
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
      regionTree: [],
      optionsProps: {
        value: "id",
        label: "name",
        children: "children",
      },
    };
  },
  components: {
    AddOrUpdate,
  },
  created() {
    this.regionTreeList();
  },
  methods: {
    handleChange(value) {
      this.dataForm.cityId = value[value.length - 1];
    },
    regionTreeList() {
      this.$http
        .get("/sys/region/tree")
        .then(({ data: res }) => {
          if (res.code !== 0) {
            return this.$message.error(res.msg);
          }
          let regionTree = treeDataTranslate(res.data);
          this.regionTree = this.delateChildren(regionTree);
          console.log(this.regionTree);
        })
        .catch(() => {});
    },
    delateChildren(arr) {
      if (arr.length) {
        for (let i in arr) {
          if (arr[i].children.length) {
            this.delateChildren(arr[i].children);
          } else {
            delete arr[i].children;
          }
        }
      }
      return arr;
    },
  },
};
</script>

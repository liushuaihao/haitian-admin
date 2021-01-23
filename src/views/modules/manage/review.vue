<template>
  <el-card shadow="never" class="aui-card--fill">
    <div class="mod-sys__role">
      <!-- <el-form
        :inline="true"
        :model="dataForm"
        @keyup.enter.native="getDataList()"
      >
        <el-form-item>
          <el-input
            v-model="dataForm.mobile"
            placeholder="手机号"
            clearable
          ></el-input>
        </el-form-item>
        <el-form-item>
          <el-button @click="getDataList()">{{ $t("query") }}</el-button>
        </el-form-item>
        <el-form-item>
          <el-button
            v-if="$hasPermission('sys:role:save')"
            type="primary"
            @click="addOrUpdateHandle()"
            >{{ $t("add") }}</el-button
          >
        </el-form-item>
      </el-form> -->
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
          prop="mobile"
          label="手机号"
          header-align="center"
          align="center"
        ></el-table-column>
        <el-table-column
          prop="idCard"
          label="身份证号"
          header-align="center"
          align="center"
        ></el-table-column>
        <el-table-column
          prop="status"
          label="状态"
          header-align="center"
          align="center"
        >
          <!-- status	0待审核 1通过 2拒绝 -->
          <template slot-scope="scope">
            <el-tag v-if="scope.row.status === 0" size="small"> 待审核</el-tag>
            <el-tag v-if="scope.row.status === 1" size="small" type="success"
              >通过</el-tag
            >
            <el-tag v-if="scope.row.status === 2" size="small" type="danger"
              >拒绝</el-tag
            >
          </template>
        </el-table-column>
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
              v-if="
                scope.row.status === 0 &&
                  $hasPermission('review:review:approve')
              "
              @click="status(scope.row.id, '1')"
              >通过</el-button
            >
            <el-button
              type="text"
              size="small"
              v-if="
                scope.row.status === 0 &&
                  $hasPermission('review:review:approve')
              "
              @click="status(scope.row.id, '2')"
              >拒绝</el-button
            >
            <el-button
              type="text"
              size="small"
              v-if="$hasPermission('review:review:info')"
              @click="addOrUpdateHandle(scope.row.id)"
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
import AddOrUpdate from "./review-add-or-update";
export default {
  mixins: [mixinViewModule],
  data() {
    return {
      mixinViewModuleOptions: {
        getDataListURL: "/manage/review/list",
        getDataListIsPage: true,
        deleteURL: "/manage/review/delete",
        deleteIsBatch: true,
      },
      dataForm: {
        mobile: "",
      },
    };
  },
  components: {
    AddOrUpdate,
  },
  methods: {
    status: function(id, status) {
      this.$confirm(
        this.$t("prompt.info", { handle: status == 1 ? "同意" : "拒绝" }),
        this.$t("prompt.title"),
        {
          confirmButtonText: this.$t("confirm"),
          cancelButtonText: this.$t("cancel"),
          type: "warning",
        }
      )
        .then((res) => {
          this.$http
            .post("/manage/review/approve", {
              id: id,
              status: status,
            })
            .then(({ data: res }) => {
              console.log(this.$t("prompt.success"));
              if (res.code !== 0) {
                return this.$message.error(res.msg);
              }
              this.$message({
                message: this.$t("prompt.success"),
                type: "success",
                duration: 500,
                onClose: () => {
                  this.query();
                },
              });
            });
        })
        .catch(() => {});
    },
  },
};
</script>

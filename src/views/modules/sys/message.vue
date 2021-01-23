<template>
  <el-card shadow="never" class="aui-card--fill">
    <div class="mod-sys__role">
      <el-form
        :inline="true"
        :model="dataForm"
        @keyup.enter.native="getDataList()"
      >
        <el-form-item>
          <el-input
            v-model="dataForm.noticeTitle"
            :placeholder="$t('message.messageName')"
            clearable
          ></el-input>
        </el-form-item>
        <el-form-item>
          <el-button @click="getDataList()">{{ $t("query") }}</el-button>
        </el-form-item>
        <el-form-item>
          <el-button
            type="primary"
            v-if="$hasPermission('sys:message:save')"
            @click="addOrUpdateHandle()"
            >{{ $t("message.add") }}</el-button
          >
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
          prop="noticeTitle"
          :label="$t('message.messageName')"
          header-align="center"
          align="center"
        ></el-table-column>
        <el-table-column
          prop="noticeObject"
          :label="$t('message.accepter')"
          header-align="center"
          align="center"
        >
          <template slot-scope="scope">
            <div>
              <span>{{ options[scope.row.noticeObject].label }}</span>
            </div>
          </template>
        </el-table-column>
        <el-table-column
          prop="createDate"
          sortable="custom"
          :label="$t('message.sendTime')"
          header-align="center"
          align="center"
          width="180"
        ></el-table-column>
        <el-table-column
          :label="$t('handle')"
          header-align="center"
          align="center"
          width="150"
        >
          <template slot-scope="scope">
            <el-button
              type="text"
              size="small"
              v-if="$hasPermission('sys:message:info')"
              @click="addOrUpdateHandle(scope.row.id)"
              >{{ $t("message.detail") }}</el-button
            >
            <el-button
              type="text"
              size="small"
              v-if="$hasPermission('sys:message:delete')"
              @click="deleteHandle(scope.row.id)"
              >{{ $t("message.delete") }}</el-button
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
      >
      </el-pagination>
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
import AddOrUpdate from "./message-add-or-update";
export default {
  mixins: [mixinViewModule],
  data() {
    return {
      mixinViewModuleOptions: {
        getDataListURL: "notice/list",
        getDataListIsPage: true,
        deleteURL: "/notice/delete",
        deleteIsBatch: true,
      },
      options: [
        {
          value: "0",
          label: "所有用户",
        },
        {
          value: "1",
          label: "医生",
        },
        {
          value: "2",
          label: "患者",
        },
        {
          value: "3",
          label: "患者及家属",
        },
      ],
    };
  },
  components: {
    AddOrUpdate,
  },
};
</script>

<template>
  <el-card shadow="never" class="aui-card--fill">
    <div class="mod-sys__user">
      <el-form
        :inline="true"
        :model="dataForm"
        @keyup.enter.native="getDataList()"
      >
        <el-form-item>
          <el-input
            v-model="dataForm.name"
            placeholder="姓名"
            clearable
          ></el-input>
        </el-form-item>
        <el-form-item>
          <el-input
            v-model="dataForm.mobile"
            placeholder="手机号"
            clearable
          ></el-input>
        </el-form-item>
        <el-form-item>
          <el-input
            v-model="dataForm.IDNumber"
            placeholder="身份证号"
            clearable
          ></el-input>
        </el-form-item>
        <el-form-item>
          <el-select v-model="role" placeholder="筛选角色">
            <el-option
              v-for="item in roleSelect"
              :key="item.value"
              :label="item.label"
              :value="item.value"
            ></el-option>
          </el-select>
        </el-form-item>
        <!-- <el-form-item>
          <ren-dept-tree v-model="dataForm.deptId" :placeholder="$t('dept.title')" :query="true"></ren-dept-tree>
        </el-form-item>-->
        <el-form-item>
          <el-button @click="getDataList()">{{ $t("query") }}</el-button>
        </el-form-item>
        <el-form-item>
          <el-button
            v-if="$hasPermission('sys:user:save')"
            type="primary"
            @click="addOrUpdateHandle()"
            >{{ $t("add") }}</el-button
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
          prop="username"
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
          prop=""
          label="身份证号"
          min-width="200px"
          header-align="center"
          align="center"
        ></el-table-column>
        <el-table-column
          prop="deptName"
          :label="$t('user.deptName')"
          header-align="center"
          align="center"
        ></el-table-column>
        <el-table-column
          prop=""
          label="角色"
          align="center"
          width="100"
        ></el-table-column>
        <el-table-column
          prop=""
          label="所属区域"
          header-align="center"
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
        <el-table-column
          prop="status"
          :label="$t('user.status')"
          sortable="custom"
          header-align="center"
          align="center"
        >
          <template slot-scope="scope">
            <el-tag v-if="scope.row.status === 0" size="small" type="danger">{{
              $t("user.status0")
            }}</el-tag>
            <el-tag v-else size="small" type="success">{{
              $t("user.status1")
            }}</el-tag>
          </template>
        </el-table-column>
        <el-table-column
          :label="$t('handle')"
          fixed="right"
          header-align="center"
          align="center"
          width="150"
        >
          <template slot-scope="scope">
            <el-button type="text" size="small">重置密码</el-button>
            <el-button
              v-if="$hasPermission('sys:user:update')"
              type="text"
              size="small"
              @click="addOrUpdateHandle(scope.row.id)"
              >{{ $t("update") }}</el-button
            >
            <el-button
              v-if="$hasPermission('sys:user:delete')"
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
import AddOrUpdate from "./user-add-or-update";
export default {
  mixins: [mixinViewModule],
  data() {
    return {
      mixinViewModuleOptions: {
        getDataListURL: "/sys/user/page",
        getDataListIsPage: true,
        deleteURL: "/sys/user",
        deleteIsBatch: true,
        exportURL: "/sys/user/export",
      },
      dataForm: {
        name: "",
        mobile: "",
        IDNumber: "",
      },
      role: "", //角色
      roleSelect: [
        {
          value: "选项1",
          label: "医生",
        },
        {
          value: "选项2",
          label: "患者",
        },
        {
          value: "选项3",
          label: "管理员",
        },
        {
          value: "选项4",
          label: "患者家属",
        },
      ],
    };
  },
  components: {
    AddOrUpdate,
  },
};
</script>

<template>
  <el-card shadow="never" class="aui-card--fill">
    <div class="mod-sys__role">
      <el-form :inline="true" :model="dataForm" @keyup.enter.native="getDataList()">
        <el-form-item>
          <el-input v-model="dataForm.mobile" placeholder="手机号" clearable></el-input>
        </el-form-item>
        <el-form-item>
          <el-button @click="getDataList()">{{ $t('query') }}</el-button>
        </el-form-item>
        <el-form-item>
          <el-button v-if="$hasPermission('sys:role:save')" type="primary" @click="addOrUpdateHandle()">{{ $t('add') }}</el-button>
        </el-form-item>
      </el-form>
      <el-table
        v-loading="dataListLoading"
        :data="dataList2"
        border
        @selection-change="dataListSelectionChangeHandle"
        @sort-change="dataListSortChangeHandle"
        style="width: 100%;"
      >
        <el-table-column
          prop="name"
          label="姓名"
          header-align="center"
          align="center"
        ></el-table-column>
        <el-table-column
          prop="mobile"
          label="电话"
          header-align="center"
          align="center"
        ></el-table-column>
        <el-table-column
          prop="identityCard"
          label="身份证号"
          header-align="center"
          align="center"
        ></el-table-column>
        <el-table-column
          :label="$t('handle')"
          fixed="right"
          header-align="center"
          align="center"
          width="150"
        >
          <template slot-scope="scope">
            <el-button type="text" size="small" @click="forwardUrl(scope.row)">修改</el-button>
            <el-button type="text" size="small" @click="forwardUrl(scope.row)">删除</el-button>
          </template>
        </el-table-column>
      </el-table>
      <!-- 弹窗, 新增 / 修改 -->
    </div>
  </el-card>
</template>

<script>
import mixinViewModule from '@/mixins/view-module'
export default {
  mixins: [mixinViewModule],
  data () {
    return {
      mixinViewModuleOptions: {
        getDataListURL: '/sys/role/page',
        getDataListIsPage: true,
        deleteURL: '/sys/role',
        deleteIsBatch: true
      },
      dataForm: {
        mobile:'',
      },
      dataList2: [
        {
          name: "张三",
          mobile: "16601285208",
          identityCard: "41102419991221771X"
        }
      ]
    }
  },
  components: {
  }
}
</script>

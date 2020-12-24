<template>
  <el-card shadow="never" class="aui-card--fill">
    <div class="mod-sys__role">
      <el-form :inline="true" :model="dataForm" @keyup.enter.native="getDataList()">
        <el-form-item>
          <el-input v-model="dataForm.office" placeholder="科室搜索" clearable></el-input>
        </el-form-item>
        <el-form-item>
          <el-input v-model="dataForm.time" placeholder="时间搜索" clearable></el-input>
        </el-form-item>
        <el-form-item>
          <el-button @click="getDataList()">{{ $t('query') }}</el-button>
        </el-form-item>
        <el-form-item>
          <el-button
            v-if="$hasPermission('sys:role:save')"
            type="primary"
            @click="dialogFormVisible = true"
          >{{ $t('add') }}</el-button>
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
        <el-table-column prop="name" label="科室名称" header-align="center" align="center"></el-table-column>
        <el-table-column prop="time" label="添加时间" header-align="center" align="center"></el-table-column>
        <el-table-column
          :label="$t('handle')"
          fixed="right"
          header-align="center"
          align="center"
          width="150"
        >
          <template slot-scope="scope">
            <el-button type="text" size="small" @click="dialogFormVisible2=true">修改</el-button>
            <el-button type="text" size="small" @click="forwardUrl(scope.row)">删除</el-button>
          </template>
        </el-table-column>
      </el-table>
      <!-- 弹窗, 新增 / 修改 -->

      <el-dialog title="新增" :visible.sync="dialogFormVisible">
        <el-form>
          <el-form-item label="科室名称" :label-width="formLabelWidth">
            <el-col :span="10">
              <el-input v-model="addtnotd" autocomplete="off"></el-input>
            </el-col>
          </el-form-item>
        </el-form>
        <div slot="footer" class="dialog-footer">
          <el-button @click="dialogFormVisible = false">取 消</el-button>
          <el-button type="primary" @click="dialogFormVisible = false">确 定</el-button>
        </div>
      </el-dialog>
      <el-dialog title="修改" :visible.sync="dialogFormVisible2">
        <el-form>
          <el-form-item label="科室名称" :label-width="formLabelWidth">
            <el-col :span="10">
              <el-input v-model="tnotd" autocomplete="off"></el-input>
            </el-col>
          </el-form-item>
        </el-form>
        <div slot="footer" class="dialog-footer">
          <el-button @click="dialogFormVisible2 = false">取 消</el-button>
          <el-button type="primary" @click="dialogFormVisible2 = false">确 定</el-button>
        </div>
      </el-dialog>
    </div>
  </el-card>
</template>

<script>
import mixinViewModule from "@/mixins/view-module";
export default {
  mixins: [mixinViewModule],
  data () {
    return {
      mixinViewModuleOptions: {
        getDataListURL: "/sys/role/page",
        getDataListIsPage: true,
        deleteURL: "/sys/role",
        deleteIsBatch: true
      },
      dataForm: {
        office: "",
        time: ""
      },
      dataList2: [
        {
          name: "张三",
          time: "2019-04-05"
        }
      ],
      dialogFormVisible: false,
      dialogFormVisible2: false,
      tnotd: "", // 科室名称
      addtnotd: "",
      formLabelWidth: "120px"
    };
  },
  components: {}
};
</script>

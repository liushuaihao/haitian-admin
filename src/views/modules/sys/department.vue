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
            v-model="dataForm.office"
            placeholder="科室搜索"
            clearable
          ></el-input>
        </el-form-item>
        <el-form-item>
          <el-input
            v-model="dataForm.time"
            placeholder="时间搜索"
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
            @click="dialogFormVisible = true"
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
        ref="table"
      >
        <el-table-column
          prop="deptName"
          label="科室名称"
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
          width="150"
        >
          <template slot-scope="scope">
            <el-button
              type="text"
              size="small"
              @click="
                (dialogFormVisible2 = true),
                  modification(scope.$index, dataList)
              "
              >修改</el-button
            >
            <el-button
              type="text"
              size="small"
              @click="deleteHandle(scope.row.id)"
              >删除</el-button
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

      <el-dialog title="新增" :visible.sync="dialogFormVisible">
        <el-form :rules="dataRule">
          <el-form-item
            label="科室名称"
            prop="deptName"
            :label-width="formLabelWidth"
          >
            <el-col :span="10">
              <el-input v-model="addtnotd" autocomplete="off"></el-input>
            </el-col>
          </el-form-item>
        </el-form>
        <div slot="footer" class="dialog-footer">
          <el-button @click="dialogFormVisible = false">取 消</el-button>
          <el-button
            type="primary"
            @click="(dialogFormVisible = false), addTnotd()"
            >确 定</el-button
          >
        </div>
      </el-dialog>
      <el-dialog title="修改" :visible.sync="dialogFormVisible2">
        <el-form :rules="dataRule">
          <el-form-item
            label="科室名称"
            prop="deptName"
            :label-width="formLabelWidth"
          >
            <el-col :span="10">
              <el-input v-model="tnotd.deptName" autocomplete="off"></el-input>
            </el-col>
          </el-form-item>
        </el-form>
        <div slot="footer" class="dialog-footer">
          <el-button @click="dialogFormVisible2 = false">取 消</el-button>
          <el-button type="primary" @click="updates()">确 定</el-button>
        </div>
      </el-dialog>
    </div>
  </el-card>
</template>

<script>
import mixinViewModule from "@/mixins/view-module";
export default {
  mixins: [mixinViewModule],
  data() {
    return {
      mixinViewModuleOptions: {
        getDataListURL: "/dept/dept/page",
        getDataListIsPage: true,
        deleteURL: "/dept/dept",
        deleteIsBatch: true,
      },
      dataForm: {
        office: "",
        time: "",
      },
      dataList2: [
        {
          name: "张三",
          time: "2019-04-05",
        },
      ],
      dialogFormVisible: false,
      dialogFormVisible2: false,
      tnotd: {}, // 科室名称
      addtnotd: "",
      formLabelWidth: "120px",
    };
  },
  components: {
    mixinViewModule,
  },
  computed: {
    dataRule() {
      return {
        deptName: [
          {
            required: true,
            message: "请输入科室名称",
            trigger: "blur",
          },
        ],
      };
    },
  },
  methods: {
    //新增
    addTnotd: function() {
      if (this.addtnotd != "") {
        this.$http
          .post("/dept/dept", { deptName: this.addtnotd })
          .then((res) => {
            this.getDataList();
          })
          .catch(() => {});
      }
    },
    modification: function(index, rows) {
      console.log(rows[index]);
      this.tnotd = {
        createDate: rows[index].createDate,
        creator: rows[index].creator,
        deptName: rows[index].deptName,
        id: rows[index].id,
        updateDate: rows[index].updateDate,
        updater: rows[index].updater,
      };
      //信息
      this.$http
        .get(`/dept/dept/${this.tnotd.id }`)
        .then((res) => {
          console.log(res);
        })
        .catch(() => {});
    },
    // 修改
    updates: function() {
      this.dialogFormVisible2 = false;
      let param = {
        deptName: this.tnotd.deptName,
        id: this.tnotd.id,
      };
      this.$http
        .put("/dept/dept", { params: param })
        .then((res) => {
          console.log(res);
        })
        .catch(() => {});
    },
  },
};
</script>

<template>
  <el-dialog
    :visible.sync="visible"
    :title="!dataForm.id ? $t('add') : $t('update')"
    :close-on-click-modal="false"
    :close-on-press-escape="false"
  >
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm">
      <el-form-item
        label="科室名称"
        prop="addtnotd"
        v-if="!dataForm.id"
        :label-width="formLabelWidth"
        clearable
      >
        <el-col :span="10">
          <el-input
            v-model="dataForm.addtnotd"
            maxlength="10"
            show-word-limit
          ></el-input>
        </el-col>
      </el-form-item>
      <el-form-item
        label="科室名称"
        prop="deptName"
        v-if="dataForm.id"
        :label-width="formLabelWidth"
        clearable
      >
        <el-col :span="10">
          <el-input
            v-model="dataForm.deptName"
            maxlength="10"
            show-word-limit
          ></el-input>
        </el-col>
      </el-form-item>
    </el-form>
    <template slot="footer" v-if="!dataForm.id">
      <el-button @click="visible = false">{{ $t("cancel") }}</el-button>
      <el-button type="primary" @click="addTnotd()">{{
        $t("confirm")
      }}</el-button>
    </template>
    <template slot="footer" v-if="dataForm.id">
      <el-button @click="visible = false">{{ $t("cancel") }}</el-button>
      <el-button type="primary" @click="updates()">{{
        $t("confirm")
      }}</el-button>
    </template>
  </el-dialog>
</template>

<script>
// import debounce from "lodash/debounce";
export default {
  data() {
    return {
      visible: false,
      deptList: [],
      deptListVisible: false,
      dataForm: {
        id: "",
        deptName: "",
        addtnotd: "",
      },
      formLabelWidth: "120px",
    };
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
        addtnotd: [
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
      this.$refs["dataForm"].validate((valid) => {
        if (!valid) {
          return false;
        }
        this.$http
          .post("/sys/dept/add", { deptName: this.dataForm.addtnotd })
          .then((res) => {
            this.visible = false;
            this.$emit("refreshDataList");
          })
          .catch(() => {});
      });
    },
    modification: function() {
      //   信息
      this.$http
        .get(`/sys/dept/getDeptDetail?id=${this.dataForm.id}`)
        .then((res) => {
          console.log(res);
          let dataList = res.data.data;
          this.dataForm.deptName = dataList.deptName;
        })
        .catch(() => {});
    },
    // 修改
    updates: function() {
      let param = {
        deptName: this.dataForm.deptName,
        id: this.dataForm.id,
      };
      this.$refs["dataForm"].validate((valid) => {
        if (!valid) {
          return false;
        }
        this.$http
          .post("/sys/dept/updateDept", {
            id: param.id,
            deptName: param.deptName,
          })
          .then((res) => {
            this.visible = false;
            this.$emit("refreshDataList");
          })
          .catch(() => {});
      });
    },
    init() {
      this.visible = true;
      this.$nextTick(() => {
        this.$refs["dataForm"].resetFields();
        if (this.dataForm.id) {
          this.modification();
        }
      });
    },
  },
};
</script>

<style lang="scss">
.mod-sys__dept {
  .dept-list {
    .el-input__inner,
    .el-input__suffix {
      cursor: pointer;
    }
  }
}
</style>

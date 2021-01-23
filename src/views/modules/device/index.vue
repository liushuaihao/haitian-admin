<template>
  <el-card shadow="never" class="aui-card--fill">
    <div class="mod-sys__dept">
      <el-form
        :inline="true"
        :model="dataForm"
        @keyup.enter.native="getDataList()"
      >
        <!-- 用户身份证/姓名/电话号/中控设备id号/测量设备ID号 -->
        <el-form-item>
          <el-input
            placeholder="用户身份证"
            v-model="dataForm.idCard"
            clearable
          ></el-input>
        </el-form-item>
        <el-form-item>
          <el-input
            placeholder="姓名"
            v-model="dataForm.realName"
            clearable
          ></el-input>
        </el-form-item>
        <el-form-item>
          <el-input
            placeholder="手机号"
            v-model="dataForm.mobile"
            clearable
          ></el-input>
        </el-form-item>
        <el-form-item>
          <el-input
            placeholder="中控设备ID号"
            v-model="dataForm.deviceId"
            clearable
          ></el-input>
        </el-form-item>
        <el-form-item>
          <el-input
            placeholder="测量设备ID号"
            v-model="dataForm.measureId"
            clearable
          ></el-input>
        </el-form-item>
        <el-form-item>
          <el-select
            v-model="dataForm.stateCode"
            placeholder="绑定状态"
            clearable
          >
            <el-option label="全部" value="quanbu"></el-option>
            <el-option label="绑定" value="bangding"></el-option>
            <el-option label="空闲" value="kongxian"></el-option>
          </el-select>
        </el-form-item>

        <el-form-item>
          <el-cascader
            style="width:100%;"
            v-model="dataForm.cityId"
            placeholder="位置属性"
            :options="regionTree"
            :props="optionsProps"
            @change="handleChange"
            clearable
          ></el-cascader>
        </el-form-item>
        <el-form-item>
          <el-select
            v-model="dataForm.monitorDevice"
            placeholder="测量设备"
            clearable
          >
            <el-option label="全部" value="0"></el-option>
            <el-option label="心电设备" value="1"></el-option>
            <el-option label="呼吸设备" value="2"></el-option>
            <el-option label="左上设备" value="3"></el-option>
            <el-option label="右上设备" value="4"></el-option>
            <el-option label="左下设备" value="5"></el-option>
            <el-option label="右下设备" value="6"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item>
          <el-button @click="getDataList()">{{ $t("query") }}</el-button>
          <el-button type="primary" @click="addOrUpdateHandle()">{{
            $t("add")
          }}</el-button>
        </el-form-item>
      </el-form>
      <!-- 设备ID 姓名 电话 身份证号 状态 时间 -->
      <el-table
        v-loading="dataListLoading"
        :data="dataList"
        row-key="id"
        border
        style="width: 100%;"
      >
        <el-table-column
          prop="deviceId"
          label="设备ID"
          header-align="center"
          align="center"
        ></el-table-column>
        <el-table-column
          prop="realName"
          label="姓名"
          header-align="center"
          align="center"
        ></el-table-column>
        <el-table-column
          prop="mobile"
          :label="$t('sms.mobile')"
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
          prop="stateLabel"
          label="绑定状态"
          header-align="center"
          align="center"
        ></el-table-column>
        <el-table-column
          prop="createDate"
          label="绑定时间"
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
              @click="infoHandle(scope.row.id)"
              >详情</el-button
            >
            <el-button
              type="text"
              size="small"
              @click="deleteHandle(scope.row.id)"
              >{{ $t("delete") }}</el-button
            >
            <el-button
              type="text"
              size="small"
              @click="unbindHandle(scope.row.id)"
              >解绑</el-button
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

      <!-- 详情 -->
      <add-or-update
        v-if="addOrUpdateVisible"
        ref="addOrUpdate"
        @refreshDataList="getDataList"
      ></add-or-update>
    </div>

    <el-dialog
      :visible.sync="addVisible"
      title="添加设备"
      :close-on-click-modal="false"
      :close-on-press-escape="false"
    >
      <el-form
        :model="addForm"
        :rules="dataRule"
        ref="addForm"
        @keyup.enter.native="dataFormSubmitHandle()"
        label-width="120px"
      >
        <el-form-item label="手机号" prop="mobile">
          <el-input
            v-model="addForm.mobile"
            maxlength="11"
            show-word-limit
          ></el-input>
        </el-form-item>
        <el-form-item label="设备ID" prop="deviceId">
          <el-input v-model="addForm.deviceId"></el-input>
        </el-form-item>
      </el-form>
      <template slot="footer">
        <el-button @click="addVisible = false">{{ $t("cancel") }}</el-button>
        <el-button type="primary" @click="dataFormSubmitHandle()">{{
          $t("confirm")
        }}</el-button>
      </template>
    </el-dialog>
  </el-card>
</template>

<script>
import debounce from "lodash/debounce";
import mixinViewModule from "@/mixins/view-module";
import AddOrUpdate from "./index-add-or-update";
import { treeDataTranslate } from "@/utils";
import { isEmail, isMobile, isIdCard } from "@/utils/validate";
export default {
  mixins: [mixinViewModule],
  data() {
    return {
      addVisible: false,
      mixinViewModuleOptions: {
        getDataListURL: "/device/devicebin/list",
        getDataListIsPage: true,
        deleteURL: "",
        deleteIsBatch: true,
      },
      regionTree: [],
      optionsProps: {
        value: "id",
        label: "name",
        children: "children",
      },
      addForm: {
        deviceId: "", //设备ID
        mobile: "", //手机号
      },
    };
  },
  computed: {
    dataRule() {
      var validateMobile = (rule, value, callback) => {
        if (value && !isMobile(value)) {
          return callback(
            new Error(
              this.$t("validate.format", { attr: this.$t("user.mobile") })
            )
          );
        }
        callback();
      };
      return {
        deviceId: [
          {
            required: true,
            message: this.$t("validate.required"),
            trigger: "blur",
          },
        ],
        mobile: [
          {
            required: true,
            validator: validateMobile,
            trigger: "blur",
          },
        ],
      };
    },
  },
  components: {
    AddOrUpdate,
  },
  created() {
    this.regionTreeList();
  },
  methods: {
    addOrUpdateHandle() {
      this.addVisible = true;
      this.$nextTick(() => {
        this.$refs["addForm"].resetFields();
      });
    },
    // 表单提交
    dataFormSubmitHandle: debounce(
      function() {
        this.$refs["addForm"].validate((valid) => {
          if (!valid) {
            return false;
          }
          this.$http["post"]("/device/devicebin/add", this.addForm)
            .then(({ data: res }) => {
              if (res.code !== 0) {
                return this.$message.error(res.msg);
              }
              this.$message({
                message: "添加成功",
                type: "success",
                duration: 500,
                onClose: () => {
                  this.addVisible = false;
                  this.getDataList();
                },
              });
            })
            .catch(() => {});
        });
      },
      1000,
      { leading: true, trailing: false }
    ),
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
    unbindHandle(id) {},
    infoHandle(id) {
      this.addOrUpdateVisible = true;
      this.$nextTick(() => {
        this.$refs.addOrUpdate.dataForm.id = id;
        this.$refs.addOrUpdate.init();
      });
    },
  },
};
</script>

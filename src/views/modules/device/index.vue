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
          <el-select
            v-model="dataForm.stateCode"
            placeholder="绑定状态"
            clearable
          >
            <el-option label="绑定" value="0"></el-option>
            <el-option label="空闲" value="1"></el-option>
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
          <!-- device:devicebin:add -->
          <el-button
            type="primary"
            v-if="$hasPermission('device:devicebin:add')"
            @click="addOrUpdateHandle()"
            >{{ $t("add") }}</el-button
          >
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
            <!-- device:devicebin:info -->
            <el-button
              type="text"
              size="small"
              v-if="
                scope.row.stateLabel == '绑定' &&
                  $hasPermission('device:devicebin:info')
              "
              @click="infoHandle(scope.row.id)"
              >详情</el-button
            >
            <!-- device:devicebin:del -->
            <el-button
              type="text"
              size="small"
              @click="deleteHandle(scope.row.id)"
              v-if="$hasPermission('device:devicebin:del')"
              >{{ $t("delete") }}</el-button
            >
            <!-- device:devicebin:unbind -->
            <el-button
              type="text"
              size="small"
              @click="unbindHandle(scope.row.id)"
              v-if="
                scope.row.stateLabel == '绑定' &&
                  $hasPermission('device:devicebin:unbind')
              "
              >解绑</el-button
            >
            <!--   v-if="$hasPermission('device:devicebin:add')" -->
            <el-button
              type="text"
              size="small"
              @click="addOrUpdateHandle(scope.row.deviceId, true)"
              v-if="
                scope.row.stateLabel == '空闲' &&
                  $hasPermission('device:devicebin:add')
              "
              >绑定</el-button
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
      :title="isUpdate ? '绑定设备' : '添加设备'"
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
        <el-form-item label="患者手机号" prop="mobile">
          <el-input
            v-model="addForm.mobile"
            maxlength="11"
            show-word-limit
          ></el-input>
        </el-form-item>
        <el-form-item label="中控设备ID" prop="deviceId">
          <el-input :disabled="isUpdate" v-model="addForm.deviceId"></el-input>
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
        deleteURL: "/device/devicebin/delete",
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
      isUpdate: false,
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
    addOrUpdateHandle(deviceId, isUpdate) {
      this.addVisible = true;
      this.isUpdate = isUpdate;
      this.$nextTick(() => {
        this.$refs["addForm"].resetFields();
        if (deviceId) {
          this.addForm.deviceId = deviceId;
        }
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
    unbindHandle(id) {
      this.$confirm(
        this.$t("prompt.info", { handle: "解绑" }),
        this.$t("prompt.title"),
        {
          confirmButtonText: this.$t("confirm"),
          cancelButtonText: this.$t("cancel"),
          type: "warning",
        }
      )
        .then(() => {
          this.$http
            .post("/device/devicebin/unBind", {
              id,
            })
            .then(({ data: res }) => {
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
            })
            .catch(() => {});
        })
        .catch(() => {});
    },
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

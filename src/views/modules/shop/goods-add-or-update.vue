<template>
  <!-- <el-dialog
    :visible.sync="visible"
    :title="!dataForm.id ? $t('add') : $t('update')"
    :close-on-click-modal="false"
    :close-on-press-escape="false"
    width="1000px"
    :close="closeVisible"
  > -->
  <el-card class="content-goods">
    <!-- {{ dataForm }} -->
    <el-form
      :model="dataForm"
      :rules="dataRule"
      ref="dataForm"
      label-width="100px"
    >
      <el-form-item prop="goodsType" label="商品类型">
        <el-select
          v-model="dataForm.goodsType"
          style="width:250px"
          placeholder="商品类型"
          clearable
        >
          <el-option label="在线服务" value="0"></el-option>
          <el-option label="设备租赁" value="1"></el-option>
          <el-option label="设备购买" value="2"></el-option>
        </el-select>
      </el-form-item>

      <el-form-item prop="goodsName" label="商品名称">
        <el-input
          v-model="dataForm.goodsName"
          type="textarea"
          maxlength="60"
          style="width:500px;min-height: 40px;overflow: hidden;"
          show-word-limit
          placeholder="商品名称"
          clearable
        ></el-input>
      </el-form-item>

      <el-row v-if="dataForm.goodsType == 0">
        <el-col :span="8">
          <el-form-item prop="orgId" label="所属机构" class="role-list">
            <el-select
              v-model="dataForm.orgId"
              style="width:250px"
              placeholder="所属机构"
              @change="orgChange"
              clearable
            >
              <el-option
                v-for="organ in organList"
                :key="organ.id"
                :label="organ.orgName"
                :value="organ.id"
              ></el-option>
            </el-select> </el-form-item
        ></el-col>
        <el-col :span="8"
          ><el-form-item prop="deptId" label="所属科室" class="role-list">
            <el-select
              v-model="dataForm.deptId"
              style="width:250px"
              placeholder="所属科室"
              @change="orgChange"
              clearable
            >
              <el-option
                v-for="dept in deptList"
                :key="dept.id"
                :label="dept.deptName"
                :value="dept.id"
              ></el-option>
            </el-select> </el-form-item
        ></el-col>
        <el-col :span="8"
          ><el-form-item prop="doctorId" label="医生" class="role-list">
            <el-select
              v-model="dataForm.doctorId"
              style="width:250px"
              placeholder="医生"
              clearable
            >
              <el-option
                v-for="doctor in doctorList"
                :key="doctor.id"
                :label="doctor.realName"
                :value="doctor.id"
              ></el-option>
            </el-select>
          </el-form-item>
        </el-col>
      </el-row>

      <el-form-item prop="picUrl" label="商品图片">
        <el-upload
          style="border: none;"
          :limit="9"
          list-type="picture-card"
          :action="uploadUrl"
          :file-list="galleryFileList"
          :on-remove="handleRemove"
          :on-exceed="uploadOverrun"
          :multiple="true"
          :on-success="goodsImgSuccess"
          accept=".jpg, .jpeg, .png, .gif"
          :before-upload="uploadBeforeUploadHandle"
          :on-preview="handlePictureCardPreview"
        >
          <i class="el-icon-plus"></i>
        </el-upload>
        <span style="color:#999999;font-size:14px"
          >建议尺寸800*800像素，最多上传9张</span
        >
        <el-dialog :visible.sync="dialogVisible">
          <img width="100%" :src="dialogImageUrl" alt />
        </el-dialog>
      </el-form-item>
      <el-form-item
        v-if="dataForm.goodsType == 2 ? false : true"
        prop="pkgList"
        :label="(dataForm.goodsType == 0 ? '服务' : '设备') + '时长'"
      >
        <el-button type="primary" style="margin-bottom:15px" @click="addPkg()"
          >添加</el-button
        >
        <el-table :data="dataForm.pkgList" border>
          <el-table-column
            :label="(dataForm.goodsType == 0 ? '服务' : '设备') + '时长'"
          >
            <template slot-scope="scope">
              <el-form-item
                class="el-m"
                :prop="'pkgList.' + scope.$index + '.pkgName'"
                :rules="dataRule.pkgName"
              >
                <el-select
                  style="width:100%"
                  v-model="scope.row.pkgName"
                  placeholder="套餐名称"
                  clearable
                >
                  <el-option
                    v-for="item in options"
                    :key="item.value"
                    :label="item.label"
                    :value="item.value"
                  >
                  </el-option>
                </el-select>
              </el-form-item>
            </template>
          </el-table-column>

          <el-table-column label="价格(元)" align="center">
            <template slot-scope="scope">
              <el-form-item
                class="el-m"
                :prop="'pkgList.' + scope.$index + '.pkgPrice'"
                :rules="dataRule.pkgPrice"
                clearable
              >
                <el-input-number
                  class="yuan_input"
                  style="width:100%"
                  placeholder="价格"
                  v-model="scope.row.pkgPrice"
                  controls-position="right"
                  :min="0"
                  :max="99999999"
                  :precision="2"
                ></el-input-number>
              </el-form-item>
            </template>
          </el-table-column>

          <el-table-column label="库存" align="center">
            <template slot-scope="scope">
              <el-form-item
                class="el-m"
                :prop="'pkgList.' + scope.$index + '.pkgStock'"
                :rules="dataRule.pkgStock"
              >
                <el-input-number
                  style="width:100%"
                  placeholder="库存"
                  v-model="scope.row.pkgStock"
                  controls-position="right"
                  :precision="0"
                  :min="0"
                  :max="99999999"
                  clearable
                ></el-input-number>
              </el-form-item>
            </template>
          </el-table-column>

          <el-table-column
            label="押金(元)"
            v-if="dataForm.goodsType == 0 ? false : true"
            align="center"
          >
            <template slot-scope="scope">
              <el-form-item
                class="el-m"
                :prop="'pkgList.' + scope.$index + '.deposit'"
                :rules="dataRule.deposit"
              >
                <el-input-number
                  class="yuan_input"
                  style="width:100%"
                  placeholder="押金"
                  v-model="scope.row.deposit"
                  controls-position="right"
                  :min="0"
                  :max="99999999"
                  :precision="2"
                  clearable
                ></el-input-number>
              </el-form-item>
            </template>
          </el-table-column>

          <el-table-column
            label="运费(元)"
            align="center"
            v-if="dataForm.goodsType == 0 ? false : true"
          >
            <template slot-scope="scope">
              <el-form-item
                class="el-m"
                :prop="'pkgList.' + scope.$index + '.transit'"
                :rules="dataRule.transit"
              >
                <el-input-number
                  class="yuan_input"
                  style="width:100%"
                  placeholder="运费"
                  v-model="scope.row.transit"
                  controls-position="right"
                  :min="0"
                  :max="99999999"
                  :precision="2"
                  clearable
                ></el-input-number>
              </el-form-item>
            </template>
          </el-table-column>

          <el-table-column
            :label="$t('handle')"
            fixed="right"
            header-align="center"
            align="center"
            width="150"
            v-if="dataForm.pkgList.length > 1"
          >
            <template slot-scope="scope">
              <el-button type="text" @click="deletePkg(scope.$index)"
                >删除</el-button
              >
            </template>
          </el-table-column>
        </el-table>
      </el-form-item>

      <template v-if="dataForm.goodsType == 2 ? true : false">
        <div>
          <el-form-item
            :prop="'pkgList.' + 0 + '.pkgPrice'"
            :rules="dataRule.pkgPrice"
            label="商品价格"
          >
            <el-input-number
              class="yuan_input"
              style="width:250px"
              placeholder="价格"
              v-model="dataForm.pkgList[0].pkgPrice"
              controls-position="right"
              :min="0"
              :max="99999999"
              :precision="2"
              clearable
            ></el-input-number>
          </el-form-item>
          <el-form-item
            :prop="'pkgList.' + 0 + '.pkgStock'"
            :rules="dataRule.pkgStock"
            label="库存"
          >
            <el-input-number
              style="width:250px"
              placeholder="库存"
              v-model="dataForm.pkgList[0].pkgStock"
              controls-position="right"
              :precision="0"
              :min="0"
              :max="99999999"
              clearable
            ></el-input-number>
          </el-form-item>
          <el-form-item
            :prop="'pkgList.' + 0 + '.transit'"
            :rules="dataRule.transit"
            label="运费"
          >
            <el-input-number
              class="yuan_input"
              style="width:250px"
              placeholder="运费"
              v-model="dataForm.pkgList[0].transit"
              controls-position="right"
              :min="0"
              :max="99999999"
              :precision="2"
              clearable
            ></el-input-number>
          </el-form-item>
        </div>
      </template>

      <el-form-item prop="detail" :label="'商品详情'">
        <!-- 富文本编辑器, 容器 -->
        <div id="J_quillEdito"></div>
        <!-- 自定义上传图片功能 (使用element upload组件) -->
        <el-upload
          :action="uploadUrl"
          :show-file-list="false"
          :before-upload="uploadBeforeUploadHandle"
          :on-success="uploadSuccessHandle"
          style="display: none;"
        >
          <el-button ref="uploadBtn" type="primary" size="small">
            {{ $t("upload.button") }}
          </el-button>
        </el-upload>
      </el-form-item>
    </el-form>
    <div class="goods-footer">
      <el-button class="f_l" @click="closeCurrentTab">{{
        $t("cancel")
      }}</el-button>
      <el-button class="f_l" type="primary" @click="dataFormSubmitHandle()">
        {{ $t("confirm") }}
      </el-button>
    </div>
  </el-card>
</template>

<script>
import Cookies from "js-cookie";
import debounce from "lodash/debounce";
import "quill/dist/quill.snow.css";
import Quill from "quill";
export default {
  data() {
    return {
      visible: false,
      dataForm: {
        id: "",
        goodsType: "0",
        goodsName: "",
        picUrl: [], // 图片
        doctorId: "", // 医生id
        orgId: "", // 机构
        deptId: "", // 科室
        detail: "", // 详情
        pkgList: [
          {
            deposit: "", //	押金
            pkgName: "1", //	套餐名称
            pkgPrice: "", //	套餐价格
            pkgStock: "", //	套餐库存
            transit: "", //	运费
          },
        ],
      },
      dialogImageUrl: "",
      dialogVisible: false,
      quillEditor: null,
      quillEditorToolbarOptions: [
        ["bold", "italic", "underline", "strike"],
        ["blockquote", "code-block", "image"],
        [{ header: 1 }, { header: 2 }],
        [{ list: "ordered" }, { list: "bullet" }],
        [{ script: "sub" }, { script: "super" }],
        [{ indent: "-1" }, { indent: "+1" }],
        [{ direction: "rtl" }],
        [{ size: ["small", false, "large", "huge"] }],
        [{ header: [1, 2, 3, 4, 5, 6, false] }],
        [{ color: [] }, { background: [] }],
        [{ font: [] }],
        [{ align: [] }],
        ["clean"],
      ],
      uploadUrl: "",
      galleryFileList: [],
      deptList: [],
      organList: [],
      doctorList: [],
      options: [
        {
          value: "1",
          label: "1个月",
        },
        {
          value: "3",
          label: "3个月",
        },
        {
          value: "6",
          label: "6个月",
        },
        {
          value: "12",
          label: "12个月",
        },
        {
          value: "36",
          label: "36个月",
        },
      ],
    };
  },
  computed: {
    dataRule() {
      var validateContent = (rule, value, callback) => {
        if (this.quillEditor.getLength() <= 1) {
          return callback(new Error(this.$t("validate.required")));
        }
        callback();
      };
      return {
        goodsName: [
          {
            required: true,
            message: this.$t("validate.required"),
            trigger: "blur",
          },
        ],
        doctorId: [
          {
            required: true,
            message: this.$t("validate.required"),
            trigger: "change",
          },
        ],
        orgId: [
          {
            required: true,
            message: this.$t("validate.required"),
            trigger: "change",
          },
        ],
        deptId: [
          {
            required: true,
            message: this.$t("validate.required"),
            trigger: "change",
          },
        ],
        picUrl: [
          {
            required: true,
            message: this.$t("validate.required"),
            trigger: "change",
          },
        ],
        pkgName: [
          {
            required: true,
            message: "套餐名称不能为空",
            trigger: "change",
          },
        ],
        pkgPrice: [
          {
            required: true,
            message: "价格不能为空",
            trigger: "blur",
          },
        ],
        pkgStock: [
          {
            required: true,
            message: "库存不能为空",
            trigger: "blur",
          },
        ],
        deposit: [
          {
            required: true,
            message: "押金不能为空",
            trigger: "blur",
          },
        ],
        transit: [
          {
            required: true,
            message: "运费不能为空",
            trigger: "blur",
          },
        ],
        detail: [
          {
            required: true,
            message: this.$t("validate.required"),
            trigger: "blur",
          },
          { validator: validateContent, trigger: "blur" },
        ],
      };
    },
  },
  created() {
    this.dataForm.id = this.$route.params.id;
    this.init();
  },
  methods: {
    orgChange() {
      this.getDoctorList();
    },
    init() {
      this.visible = true;
      this.$nextTick(() => {
        if (this.quillEditor) {
          this.quillEditor.deleteText(0, this.quillEditor.getLength());
        } else {
          this.quillEditorHandle();
        }
        this.$refs["dataForm"].resetFields();
        Promise.all([this.getDeptList(), this.getOrganList()]).then(() => {
          console.log(this.dataForm.id);
          if (this.dataForm.id) {
            this.getInfo();
          } else {
            this.dataForm = {
              goodsType: "0",
              goodsName: undefined,
              picUrl: [], // 图片
              doctorId: undefined, // 医生id
              orgId: undefined, // 机构
              deptId: undefined, // 科室
              detail: undefined, // 详情
              pkgList: [
                {
                  deposit: undefined, //	押金
                  pkgName: "1", //	套餐名称
                  pkgPrice: undefined, //	套餐价格
                  pkgStock: undefined, //	套餐库存
                  transit: undefined, //	运费
                },
              ],
            };
            this.galleryFileList = [];
            this.$set(this.dataForm, "deptId", "");
            this.$set(this.dataForm, "orgId", "");
            this.$set(this.dataForm, "doctorId", "");
          }
        });
      });
    },
    closeVisible() {
      this.$nextTick(() => {
        this.$refs["dataForm"].resetFields();
      });
    },
    deletePkg(index) {
      this.dataForm.pkgList.splice(index, 1);
    },
    addPkg() {
      this.dataForm.pkgList.push({
        deposit: "", //	押金
        pkgName: "", //	套餐名称
        pkgPrice: "", //	套餐价格
        pkgStock: "", //	套餐库存
        transit: "", //	运费
      });
    },
    getDoctorList(val = true) {
      let orgId = this.dataForm.orgId;
      let deptId = this.dataForm.deptId;
      if (orgId && deptId) {
        this.$http
          .get("/sys/user/searchDoctor", {
            params: {
              orgId: orgId,
              deptId: deptId,
            },
          })
          .then(({ data: res }) => {
            if (res.code !== 0) {
              return this.$message.error(res.msg);
            }
            this.doctorList = res.data;
            if (val) {
              this.$set(this.dataForm, "doctorId", "");
            }
          })
          .catch(() => {});
      }
    },
    // 获取 科室管理
    getDeptList() {
      return this.$http
        .get("/sys/dept/list/all")
        .then(({ data: res }) => {
          if (res.code !== 0) {
            return this.$message.error(res.msg);
          }
          this.deptList = res.data;
        })
        .catch(() => {});
    },
    // 获取 机构管理
    getOrganList() {
      return this.$http
        .get("/sys/org/list/all")
        .then(({ data: res }) => {
          if (res.code !== 0) {
            return this.$message.error(res.msg);
          }
          this.organList = res.data;
        })
        .catch(() => {});
    },
    // 富文本编辑器
    quillEditorHandle() {
      this.quillEditor = new Quill("#J_quillEdito", {
        modules: {
          toolbar: this.quillEditorToolbarOptions,
        },
        theme: "snow",
      });
      // 自定义上传图片功能 (使用element upload组件)
      this.uploadUrl = `${
        window.SITE_CONFIG["apiURL"]
      }/file/upload?type=4&token=${Cookies.get("token")}`;
      this.quillEditor.getModule("toolbar").addHandler("image", () => {
        this.$refs.uploadBtn.$el.click();
      });
      // 监听内容变化，动态赋值
      this.quillEditor.on("text-change", () => {
        this.dataForm.detail = this.quillEditor.root.innerHTML;
      });
    },
    // 上传图片之前
    uploadBeforeUploadHandle(file) {
      if (
        file.type !== "image/jpg" &&
        file.type !== "image/jpeg" &&
        file.type !== "image/png" &&
        file.type !== "image/gif"
      ) {
        this.$message.error(this.$t("upload.tip", { format: "jpg、png、gif" }));
        return false;
      }
    },
    // 上传图片成功
    uploadSuccessHandle(res, file, fileList) {
      if (res.code !== 0) {
        return this.$message.error(res.msg);
      }
      this.quillEditor.insertEmbed(
        this.quillEditor.getSelection().index,
        "image",
        res.data.src
      );
    },
    // 获取信息
    getInfo() {
      this.$http
        .get(`/shop/goods/getGoodsDetail?id=${this.dataForm.id}`)
        .then(({ data: res }) => {
          if (res.code !== 0) {
            return this.$message.error(res.msg);
          }
          this.dataForm = {
            ...this.dataForm,
            ...res.data,
          };
          this.quillEditor.root.innerHTML = this.dataForm.detail;
          this.galleryFileList = [];
          for (var i = 0; i < this.dataForm.picUrl.length; i++) {
            this.galleryFileList.push({
              url: this.dataForm.picUrl[i],
            });
          }
          if (this.dataForm.extendAttrs) {
            this.dataForm.deptId = this.dataForm.extendAttrs.deptId; // 科室
            this.dataForm.orgId = this.dataForm.extendAttrs.orgId; // 机构
            this.$nextTick(() => {
              this.getDoctorList(false);
              this.$set(
                this.dataForm,
                "doctorId",
                this.dataForm.extendAttrs.doctorId
              );
              this.dataForm.doctorId = this.dataForm.extendAttrs.doctorId; // 医生id
            });
          }
        })
        .catch(() => {});
    },
    // 表单提交
    dataFormSubmitHandle: debounce(
      function() {
        this.$refs["dataForm"].validate((valid) => {
          if (!valid) {
            return false;
          }
          // 在线服务 0
          let orgId =
              this.dataForm.goodsType == 0 ? this.dataForm.orgId : undefined, // 机构
            deptId =
              this.dataForm.goodsType == 0 ? this.dataForm.deptId : undefined, // 科室
            doctorId =
              this.dataForm.goodsType == 0 ? this.dataForm.doctorId : undefined; // 医生id
          let dataForm = {
            id: this.dataForm.id,
            goodsType: this.dataForm.goodsType,
            goodsName: this.dataForm.goodsName,
            picUrl: this.dataForm.picUrl, // 图片
            detail: this.dataForm.detail, // 详情
            pkgList: this.dataForm.pkgList,
            orgId: orgId,
            deptId: deptId,
            doctorId: doctorId,
          };

          this.$http["post"](
            !this.dataForm.id ? "/shop/goods/add" : "/shop/goods/updateGoods",
            dataForm
          )
            .then(({ data: res }) => {
              if (res.code !== 0) {
                return this.$message.error(res.msg);
              }
              this.$message({
                message: !this.dataForm.id ? "添加成功" : "修改成功",
                type: "success",
                duration: 500,
                onClose: () => {
                  this.closeCurrentTab();
                  this.visible = false;
                  this.$emit("refreshDataList");
                },
              });
            })
            .catch(() => {});
        });
      },
      1000,
      { leading: true, trailing: false }
    ),
    handleRemove: function(file, fileList) {
      for (var i = 0; i < this.dataForm.picUrl.length; i++) {
        // 这里存在两种情况
        // 1. 如果所删除图片是刚刚上传的图片,那么图片地址是file.response.data.url
        //    此时的file.url虽然存在,但是是本机地址,而不是远程地址。
        // 2. 如果所删除图片是后台返回的已有图片,那么图片地址是file.url
        var url;
        if (file.response === undefined) {
          url = file.url;
        } else {
          url = file.response.data.url;
        }

        if (this.dataForm.picUrl[i] === url) {
          this.dataForm.picUrl.splice(i, 1);
        }
      }
    },
    uploadOverrun: function() {
      this.$message({
        type: "error",
        message: "上传文件个数超出限制!最多上传9张图片!",
      });
    },
    handlePictureCardPreview(file) {
      this.dialogImageUrl = file.url;
      this.dialogVisible = true;
    },
    // 商品图片上传成功
    goodsImgSuccess(res, file, fileList) {
      if (res.code !== 0) {
        return this.$message.error(res.msg);
      }
      this.dataForm.picUrl.push(res.data.src);
    },

    // 关闭当前窗口
    closeCurrentTab(data) {
      var tabName = this.$store.state.contentTabsActiveName;
      this.$store.state.contentTabs = this.$store.state.contentTabs.filter(
        (item) => item.name !== tabName
      );
      if (this.$store.state.contentTabs.length <= 0) {
        this.$store.state.sidebarMenuActiveName = this.$store.state.contentTabsActiveName =
          "home";
        return false;
      }
      if (tabName === this.$store.state.contentTabsActiveName) {
        this.$router.push({
          name: "shop-goods",
        });
      }
    },
  },
};
</script>

<style lang="scss" scope>
.mod-sys__dept {
  .dept-list {
    .el-input__inner,
    .el-input__suffix {
      cursor: pointer;
    }
  }
}
.el-m {
  padding-top: 15px;
  padding-bottom: 15px;
}
.content-goods {
  position: relative;
}
.goods-footer {
  width: 100%;
  overflow: hidden;
  .f_l {
    float: right !important;
    margin-left: 20px;
  }
}
.yuan_input {
  .el-input-number__decrease,
  .el-input-number__increase {
    display: none !important;
  }
  ::before {
    content: "元";
    position: absolute;
    width: 40px;
    right: 1px;
    bottom: 1px;
    top: auto;
    left: auto;
    border-right: none;
    border-left: 1px solid #dcdfe6;
    border-radius: 0 0 4px;
    text-align: center;
    background-color: #f5f7fa;
    color: #909399;
  }
}
</style>

<template>
  <el-dialog
    :visible.sync="visible"
    :title="!dataForm.id ? $t('add') : $t('update')"
    :close-on-click-modal="false"
    :close-on-press-escape="false"
    width="1000px"
    :close="closeVisible"
  >
    <!-- {{ dataForm }} -->
    <el-form
      :model="dataForm"
      :rules="dataRule"
      ref="dataForm"
      @keyup.enter.native="dataFormSubmitHandle()"
      label-width="100px"
    >
      <el-form-item prop="goodsType" label="商品类型">
        <el-select v-model="dataForm.goodsType" placeholder="商品类型">
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
          show-word-limit
          placeholder="商品名称"
        ></el-input>
      </el-form-item>

      <el-form-item prop="doctorId" label="医生" class="role-list">
        <el-select v-model="dataForm.doctorId" placeholder="医生">
          <el-option
            v-for="doctor in doctorList"
            :key="doctor.id"
            :label="doctor.realName"
            :value="doctor.id"
          ></el-option>
        </el-select>
      </el-form-item>

      <el-form-item prop="deptId" label="所属科室" class="role-list">
        <el-select v-model="dataForm.deptId" placeholder="所属科室">
          <el-option
            v-for="dept in deptList"
            :key="dept.id"
            :label="dept.deptName"
            :value="dept.id"
          ></el-option>
        </el-select>
      </el-form-item>
      <el-form-item prop="orgId" label="所属机构" class="role-list">
        <el-select v-model="dataForm.orgId" placeholder="所属机构">
          <el-option
            v-for="organ in organList"
            :key="organ.id"
            :label="organ.orgName"
            :value="organ.id"
          ></el-option>
        </el-select>
      </el-form-item>

      <el-form-item prop="goodsName" label="商品图片">
        <el-upload
          style="border: none;"
          :limit="10"
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
        <el-dialog :visible.sync="dialogVisible">
          <img width="100%" :src="dialogImageUrl" alt />
        </el-dialog>
      </el-form-item>
      <el-form-item prop="pkgList" label="套餐详情">
        <el-row v-for="(item, index) in dataForm.pkgList" :key="index">
          <el-col :span="20">
            <div>
              <el-card style="width:400px;margin-bottom:20px">
                <el-form-item
                  label="套餐名称"
                  :prop="'pkgList.' + index + '.pkgName'"
                  :rules="{
                    required: true,
                    message: '套餐名称不能为空',
                    trigger: 'change',
                  }"
                >
                  <el-select
                    style="width:100%"
                    v-model="item.pkgName"
                    placeholder="套餐名称"
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
                <el-form-item
                  label="价格"
                  class="el-m"
                  :prop="'pkgList.' + index + '.pkgPrice'"
                  :rules="{
                    required: true,
                    message: '价格不能为空',
                    trigger: 'blur',
                  }"
                >
                  <div style="width:100%">
                    <el-input-number
                      style="width:100%"
                      placeholder="价格"
                      v-model="item.pkgPrice"
                      controls-position="right"
                      :min="0"
                      :max="99999999"
                      :precision="2"
                    >
                      <template slot="append">元</template>
                    </el-input-number>
                  </div>
                </el-form-item>
                <el-form-item
                  label="库存"
                  class="el-m"
                  :prop="'pkgList.' + index + '.pkgStock'"
                  :rules="{
                    required: true,
                    message: '库存不能为空',
                    trigger: 'blur',
                  }"
                >
                  <div style="width:100%">
                    <el-input-number
                      style="width:100%"
                      placeholder="库存"
                      v-model="item.pkgStock"
                      controls-position="right"
                      :precision="0"
                      :min="0"
                      :max="99999999"
                    ></el-input-number>
                  </div>
                </el-form-item>

                <el-form-item
                  label="押金"
                  class="el-m"
                  :prop="'pkgList.' + index + '.deposit'"
                  :rules="{
                    required: true,
                    message: '押金不能为空',
                    trigger: 'blur',
                  }"
                >
                  <div style="width:100%">
                    <el-input-number
                      style="width:100%"
                      placeholder="押金"
                      v-model="item.deposit"
                      controls-position="right"
                      :min="0"
                      :max="99999999"
                      :precision="2"
                    ></el-input-number>
                  </div>
                </el-form-item>

                <el-form-item
                  label="运费"
                  class="el-m"
                  :prop="'pkgList.' + index + '.transit'"
                  :rules="{
                    required: true,
                    message: '运费不能为空',
                    trigger: 'blur',
                  }"
                >
                  <div style="width:100%">
                    <el-input-number
                      style="width:100%"
                      placeholder="运费"
                      v-model="item.transit"
                      controls-position="right"
                      :min="0"
                      :max="99999999"
                      :precision="2"
                    ></el-input-number>
                  </div>
                </el-form-item>
              </el-card>
            </div>
          </el-col>
          <el-col :span="4">
            <el-form-item>
              <el-button type="text" @click="addPkg()">添加</el-button>
              <el-button
                type="text"
                v-if="dataForm.pkgList.length > 1"
                @click="deletePkg(index)"
                >删除</el-button
              >
            </el-form-item>
          </el-col>
        </el-row>
      </el-form-item>
      <!-- <el-form-item prop="goodsName" label="商品单价">
        <el-input
          v-model="dataForm.price"
          placeholder="商品单价"
          type="number"
        ></el-input>
      </el-form-item>
      <el-form-item prop="goodsName" label="商品说明">
        <el-input v-model="dataForm.explain" placeholder="商品说明"></el-input>
      </el-form-item>
      <el-form-item prop="goodsName" label="库存数量">
        <el-input
          v-model="dataForm.quantity"
          placeholder="库存数量"
          type="number"
        ></el-input>
      </el-form-item> -->
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
    <template slot="footer">
      <el-button @click="visible = false">{{ $t("cancel") }}</el-button>
      <el-button type="primary" @click="dataFormSubmitHandle()">
        {{ $t("confirm") }}
      </el-button>
    </template>
  </el-dialog>
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
        goodsName: "",
        picUrl: [], // 图片
        doctorId: "", // 医生id
        orgId: "", // 机构
        deptId: "", // 科室
        detail: "", // 详情
        pkgList: [
          {
            deposit: "", //	押金
            pkgName: "", //	套餐名称
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
        pkgList: [
          {
            required: true,
            message: this.$t("validate.required"),
            trigger: "blur",
          },
          { validator: validateContent, trigger: "blur" },
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
  methods: {
    init() {
      this.visible = true;
      this.$nextTick(() => {
        if (this.quillEditor) {
          this.quillEditor.deleteText(0, this.quillEditor.getLength());
        } else {
          this.quillEditorHandle();
        }
        this.$refs["dataForm"].resetFields();
        this.galleryFileList = [];
        this.dataForm.picUrl = [];
        this.$set(this.dataForm, "deptId", "");
        this.$set(this.dataForm, "orgId", "");
        Promise.all([
          this.getDeptList(),
          this.getOrganList(),
          this.getDoctorList(),
        ]).then(() => {
          console.log(this.dataForm.id);
          if (this.dataForm.id) {
            this.getInfo();
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
    getDoctorList() {
      return this.$http
        .get("/sys/user/page?userType=1&page=1&limit=1000")
        .then(({ data: res }) => {
          if (res.code !== 0) {
            return this.$message.error(res.msg);
          }
          this.doctorList = res.data.list;
        })
        .catch(() => {});
    },
    // 获取 科室管理
    getDeptList() {
      return this.$http
        .get("/sys/dept/list")
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
        .get("/sys/org/list")
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
        .get(`/shop/goods/get?id=${this.dataForm.id}`)
        .then(({ data: res }) => {
          if (res.code !== 0) {
            return this.$message.error(res.msg);
          }
          this.dataForm = {
            ...this.dataForm,
            ...res.data,
          };
          this.$nextTick(() => {
            if (this.dataForm.extendAttrs) {
              this.dataForm.doctorId = this.dataForm.extendAttrs.doctorId; // 医生id
              this.dataForm.orgId = this.dataForm.extendAttrs.orgId; // 机构
              this.dataForm.deptId = this.dataForm.extendAttrs.deptId; // 科室
            }
            this.quillEditor.root.innerHTML = this.dataForm.detail;
            this.galleryFileList = [];
            for (var i = 0; i < this.dataForm.picUrl.length; i++) {
              this.galleryFileList.push({
                url: this.dataForm.picUrl[i],
              });
            }
          });

          console.log(this.dataForm);
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
          this.$http["post"](
            !this.dataForm.id ? "/shop/goods/add" : "/shop/goods/edit",
            this.dataForm
          )
            .then(({ data: res }) => {
              if (res.code !== 0) {
                return this.$message.error(res.msg);
              }
              this.$message({
                message: this.$t("prompt.success"),
                type: "success",
                duration: 500,
                onClose: () => {
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
        message: "上传文件个数超出限制!最多上传10张图片!",
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
.el-m {
  margin-top: 15px;
}
</style>

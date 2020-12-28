<template>
  <el-dialog
    :visible.sync="visible"
    :title="!dataForm.id ? $t('add') : $t('update')"
    :close-on-click-modal="false"
    :close-on-press-escape="false"
    style="min-width:1200px"
  >
    <el-form
      :model="dataForm"
      :rules="dataRule"
      ref="dataForm"
      @keyup.enter.native="dataFormSubmitHandle()"
      label-width="100px"
    >
      <el-form-item prop="goodsName" label="服务类型">
        <el-select v-model="dataForm.type" placeholder="请选择服务类型">
          <el-option label="在线服务" value="0"></el-option>
          <el-option label="设备购买" value="1"></el-option>
          <el-option label="设备租赁" value="2"></el-option>
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
      <el-form-item prop="goodsName" label="商品图片">
        <el-upload
          action="https://jsonplaceholder.typicode.com/posts/"
          list-type="picture-card"
          :on-preview="handlePictureCardPreview"
          :on-remove="handleRemove"
        >
          <i class="el-icon-plus"></i>
        </el-upload>
        <el-dialog :visible.sync="dialogVisible">
          <img width="100%" :src="dialogImageUrl" alt />
        </el-dialog>
      </el-form-item>
      <el-form-item prop="goodsName" label="服务时长">
        <el-row>
          <el-col :span="20">
            <div>
              <el-card
                style="width:240px"
                v-for="(item, index) in service"
                :key="index"
              >
                <el-form-item prop="time">
                  <el-select
                    style="width:200px"
                    placeholder="服务时长"
                    v-model="service.time"
                  >
                    <el-option label="1个月" value="0"></el-option>
                    <el-option label="3个月" value="1"></el-option>
                    <el-option label="6个月" value="2"></el-option>
                    <el-option label="12个月" value="3"></el-option>
                  </el-select>
                </el-form-item>
                <el-form-item class="el-m" prop="jiage">
                  <div style="width:200px">
                    <el-input
                      placeholder="价格"
                      type="number"
                      v-model="service.jiage"
                    >
                      <template slot="append">元</template>
                    </el-input>
                  </div>
                </el-form-item>
                <el-form-item class="el-m" prop="kucun">
                  <div style="width:200px">
                    <el-input
                      placeholder="库存"
                      type="number"
                      v-model="service.kucun"
                    ></el-input>
                  </div>
                </el-form-item>
              </el-card>
            </div>
          </el-col>
          <el-col :span="4">
            <el-button @click="addService()">添加</el-button>
          </el-col>
        </el-row>
      </el-form-item>
      <el-form-item prop="goodsName" label="商品单价">
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
      </el-form-item>
      <el-form-item prop="content" :label="$t('mail.content')">
        <!-- 富文本编辑器, 容器 -->
        <div id="J_quillEditor"></div>
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
        price: "", // 单价
        explain: "", // 说明
        picture: "", // 图片
        type: "", // 类型
        duration: "", // 时长
        quantity: "", // 库存数量
      },
      service: [{ time: "1", jiage: "1", kucun: "1" }],
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
        content: [
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
        if (this.dataForm.id) {
          this.getInfo();
        }
      });
    },
    addService() {
      this.service.push({ time: "", jiage: "", kucun: "" });
    },
    // 富文本编辑器
    quillEditorHandle() {
      this.quillEditor = new Quill("#J_quillEditor", {
        modules: {
          toolbar: this.quillEditorToolbarOptions,
        },
        theme: "snow",
      });
      // 自定义上传图片功能 (使用element upload组件)
      this.uploadUrl = `${
        window.SITE_CONFIG["apiURL"]
      }/sys/oss/upload?token=${Cookies.get("token")}`;
      this.quillEditor.getModule("toolbar").addHandler("image", () => {
        this.$refs.uploadBtn.$el.click();
      });
      // 监听内容变化，动态赋值
      this.quillEditor.on("text-change", () => {
        this.dataForm.content = this.quillEditor.root.innerHTML;
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
    getInfo() {},
    // 表单提交
    dataFormSubmitHandle: debounce(
      function() {
        this.$refs["dataForm"].validate((valid) => {
          if (!valid) {
            return false;
          }
          this.$http[!this.dataForm.id ? "post" : "put"]("", this.dataForm)
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
    handleRemove(file, fileList) {
      console.log(file, fileList);
    },
    handlePictureCardPreview(file) {
      this.dialogImageUrl = file.url;
      this.dialogVisible = true;
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

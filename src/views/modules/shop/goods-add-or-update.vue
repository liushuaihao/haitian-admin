<template>
  <el-dialog
    :visible.sync="visible"
    :title="!dataForm.id ? $t('add') : $t('update')"
    :close-on-click-modal="false"
    :close-on-press-escape="false"
  >
    <el-form
      :model="dataForm"
      :rules="dataRule"
      ref="dataForm"
      @keyup.enter.native="dataFormSubmitHandle()"
      label-width="120px"
    >
      <el-form-item prop="goodsName" label="服务类型">
        <el-select v-model="dataForm.type" placeholder="请选择活动区域">
          <el-option label="区域一" value="shanghai"></el-option>
          <el-option label="区域二" value="beijing"></el-option>
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
        <el-card>
          <div>
            <el-form-item label="服务时长">
              <el-select v-model="dataForm.price" placeholder="服务时长">
                <el-option label="1个月" value="0"></el-option>
                <el-option label="3个月" value="1"></el-option>
                <el-option label="6个月" value="2"></el-option>
                <el-option label="12个月" value="3"></el-option>
              </el-select>
            </el-form-item>
            <el-form-item label="价格" class="el-m">
              <div style="width:260px">
                <el-input placeholder="价格" type="number" v-model="input2">
                  <template slot="append">元</template>
                </el-input>
              </div>
            </el-form-item>
            <el-form-item label="库存" class="el-m">
              <div style="width:260px">
                <el-input placeholder="库存" type="number" v-model="input2"></el-input>
              </div>
            </el-form-item>
          </div>
        </el-card>
      </el-form-item>
      <el-form-item prop="goodsName" label="商品单价">
        <el-input v-model="dataForm.price" placeholder="商品单价" type="number"></el-input>
      </el-form-item>
      <el-form-item prop="goodsName" label="商品说明">
        <el-input v-model="dataForm.explain" placeholder="商品说明"></el-input>
      </el-form-item>
      <el-form-item prop="goodsName" label="库存数量">
        <el-input v-model="dataForm.quantity" placeholder="库存数量" type="number"></el-input>
      </el-form-item>
    </el-form>
    <template slot="footer">
      <el-button @click="visible = false">{{ $t('cancel') }}</el-button>
      <el-button type="primary" @click="dataFormSubmitHandle()">{{ $t('confirm') }}</el-button>
    </template>
  </el-dialog>
</template>

<script>
import debounce from "lodash/debounce";
export default {
  data () {
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
        quantity: "" // 库存数量
      },
      dialogImageUrl: "",
      dialogVisible: false
    };
  },
  computed: {
    dataRule () {
      return {
        goodsName: [
          {
            required: true,
            message: this.$t("validate.required"),
            trigger: "blur"
          }
        ]
      };
    }
  },
  methods: {
    init () {
      this.visible = true;
      this.$nextTick(() => {
        this.$refs["dataForm"].resetFields();
        if (this.dataForm.id) {
        }
      });
    },
    // 获取信息
    getInfo () {},
    // 表单提交
    dataFormSubmitHandle: debounce(
      function () {
        this.$refs["dataForm"].validate(valid => {
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
                }
              });
            })
            .catch(() => {});
        });
      },
      1000,
      { leading: true, trailing: false }
    ),
    handleRemove (file, fileList) {
      console.log(file, fileList);
    },
    handlePictureCardPreview (file) {
      this.dialogImageUrl = file.url;
      this.dialogVisible = true;
    }

  }
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

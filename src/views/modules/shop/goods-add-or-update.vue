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
      <el-form-item prop="goodsName" label="商品名称">
        <el-input v-model="dataForm.goodsName" placeholder="商品名称"></el-input>
      </el-form-item>
      <el-form-item prop="goodsName" label="商品单价">
        <el-input v-model="dataForm.price" placeholder="商品单价" type="number"></el-input>
      </el-form-item>
      <el-form-item prop="goodsName" label="商品说明">
        <el-input v-model="dataForm.explain" placeholder="商品说明"></el-input>
      </el-form-item>
      <el-form-item prop="goodsName" label="商品图片">
      </el-form-item>
      <el-form-item prop="goodsName" label="服务类型">
        <el-select v-model="dataForm.type" placeholder="请选择活动区域">
          <el-option label="区域一" value="shanghai"></el-option>
          <el-option label="区域二" value="beijing"></el-option>
        </el-select>
      </el-form-item>
      <el-form-item prop="goodsName" label="租赁时长">
        <el-select v-model="dataForm.duration" placeholder="请选择活动区域">
          <el-option label="区域一" value="shanghai"></el-option>
          <el-option label="区域二" value="beijing"></el-option>
        </el-select>
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
      }
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
    )
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
</style>

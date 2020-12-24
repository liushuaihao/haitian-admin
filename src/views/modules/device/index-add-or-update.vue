<template>
	<el-dialog :visible.sync="visible" title="设备详情" :close-on-click-modal="false" :close-on-press-escape="false">
		<el-form :model="dataForm" ref="dataForm" @keyup.enter.native="dataFormSubmitHandle()" label-width="120px">
			<el-form-item label="账户">
				<el-input v-model="dataForm.name"></el-input>
			</el-form-item>
			<el-form-item label="姓名">
				<el-input v-model="dataForm.mobile"></el-input>
			</el-form-item>
			<el-form-item label="年龄">
				<el-input v-model="dataForm.mobile"></el-input>
			</el-form-item>
			<el-form-item label="性别">
				<el-input v-model="dataForm.mobile"></el-input>
			</el-form-item>
			<el-form-item label="电话">
				<el-input v-model="dataForm.mobile"></el-input>
			</el-form-item>
			<el-form-item label="身份证号">
				<el-input v-model="dataForm.mobile"></el-input>
			</el-form-item>
			<el-form-item label="已绑定设备ID">
				<el-input v-model="dataForm.mobile"></el-input>
			</el-form-item>
			<el-form-item label="GPS定位">
				<el-input v-model="dataForm.mobile"></el-input>
			</el-form-item>
			<p>所附带其他测量设备详情</p>
			<el-form-item label="设备通信号">
				<el-input v-model="dataForm.mobile"></el-input>
			</el-form-item>
			<el-form-item label="心电设备">
				<el-switch v-model="dataForm.xd"> </el-switch>
			</el-form-item>
			<el-form-item label="呼吸设备">
				<el-switch v-model="dataForm.hx"> </el-switch>
			</el-form-item>
			<el-form-item label="上肢设备">
				<el-switch v-model="dataForm.sz"> </el-switch>
			</el-form-item>
			<el-form-item label="下肢设备">
				<el-switch v-model="dataForm.xz"> </el-switch>
			</el-form-item>
			<el-form-item label="中控电量">
				<el-input v-model="dataForm.mobile"></el-input>
			</el-form-item>
			<el-form-item label="上肢设备电量">
				<el-input v-model="dataForm.mobile"></el-input>
			</el-form-item>
			<el-form-item label="下肢设备电量">
				<el-input v-model="dataForm.mobile"></el-input>
			</el-form-item>
		</el-form>
		<template slot="footer">
			<el-button type="primary" @click="visible = false">返回</el-button>
			<!-- <el-button type="primary" @click="dataFormSubmitHandle()">{{ $t('confirm') }}</el-button> -->
		</template>
	</el-dialog>
</template>

<script>
import debounce from 'lodash/debounce'
export default {
  data () {
    return {
      visible: false,
      dataForm: {
        id: '',
        sort: 0,
        mobile: '',
      },
    }
  },
  methods: {
    init () {
      this.visible = true
      this.$nextTick(() => {
        this.$refs['dataForm'].resetFields()
        if (this.dataForm.id) {
        }
      })
    },
    // 获取信息
    getInfo () {},
    // 表单提交
    dataFormSubmitHandle: debounce(
      function () {
        this.$refs['dataForm'].validate(valid => {
          if (!valid) {
            return false
          }
          this.$http[!this.dataForm.id ? 'post' : 'put']('', this.dataForm)
            .then(({ data: res }) => {
              if (res.code !== 0) {
                return this.$message.error(res.msg)
              }
              this.$message({
                message: this.$t('prompt.success'),
                type: 'success',
                duration: 500,
                onClose: () => {
                  this.visible = false
                  this.$emit('refreshDataList')
                },
              })
            })
            .catch(() => {})
        })
      },
      1000,
      { leading: true, trailing: false }
    ),
  },
}
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

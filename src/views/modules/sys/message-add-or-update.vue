<!--
 * @Author: tb659
 * @Date: 2020-12-24 23:00:44
 * @LastEditors: tb659
 * @LastEditTime: 2020-12-24 23:34:49
 * @Description: 
 * @FilePath: \wearable-admin\src\views\modules\sys\message-add-or-update.vue
-->
<template>
	<el-dialog :visible.sync="visible" :title="!dataForm.id ? $t('add') : $t('message.detail')" :close-on-click-modal="false" :close-on-press-escape="false">
		<el-form :model="dataForm" ref="dataForm" @keyup.enter.native="dataFormSubmitHandle()" label-width="120px">
			<el-form-item prop="messageName" :label="$t('message.messageName')">
				<el-input v-if="!dataForm.id" v-model="dataForm.messageName" :placeholder="$t('message.messageName')"></el-input>
				<span v-else>{{ dataForm.messageName }}</span>
			</el-form-item>
			<el-form-item prop="sendPeople" :label="$t('message.sendPeople')">
				<el-select v-if="!dataForm.id" v-model="dataForm.sendPeople" placeholder="请选择">
					<el-option v-for="item in options" :key="item.label" :label="item.label" :value="item.value"> </el-option>
				</el-select>
				<span v-else>{{ dataForm.sendPeople }}</span>
			</el-form-item>
			<el-form-item prop="sendMsg" :label="$t('message.sendMsg')">
				<el-input v-if="!dataForm.id" type="textarea" v-model="dataForm.sendMsg" :placeholder="$t('message.sendMsg')"></el-input>
				<span v-else>{{ dataForm.sendMsg }}</span>
			</el-form-item>
		</el-form>
		<template slot="footer">
			<el-button @click="visible = false">{{ $t('cancel') }}</el-button>
			<el-button type="primary" @click="dataFormSubmitHandle()">{{ $t('confirm') }}</el-button>
		</template>
	</el-dialog>
</template>

<script>
import debounce from 'lodash/debounce'
export default {
	data() {
		return {
			visible: false,
			menuList: [],
			deptList: [],
			dataForm: {
				id: '',
				messageName: '活动',
				sendPeople: '医生',
				sendMsg: '消息内容',
			},
			options: [
				{
					value: '1',
					label: '医生',
				},
				{
					value: '2',
					label: '患者',
				},
				{
					value: '3',
					label: '患者及家属',
				},
				{
					value: '4',
					label: '所有用户',
				},
			],
		}
	},
	computed: {
		dataRule() {
			return {}
		},
	},
	methods: {
		init() {
			this.visible = true
			this.$nextTick(() => {
				this.$refs['dataForm'].resetFields()
			})
		},
		// 表单提交
		dataFormSubmitHandle: debounce(
			function() {
				this.$refs['dataForm'].validate(valid => {
					if (!valid) {
						return false
					}
					this.dataForm.menuIdList = [...this.$refs.menuListTree.getHalfCheckedKeys(), ...this.$refs.menuListTree.getCheckedKeys()]
					this.dataForm.deptIdList = this.$refs.deptListTree.getCheckedKeys()
					this.$http[!this.dataForm.id ? 'post' : 'put']('/sys/message', this.dataForm)
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
<style lang="scss" scoped>
  .el-select {
    width: 100%;
  }
</style>

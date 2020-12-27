<template>
	<el-card shadow="never" class="aui-card--fill">
		<div class="mod-sys__role">
			<el-form :inline="true" :model="dataForm" @keyup.enter.native="getDataList()">
				<el-form-item>
					<el-input v-model="dataForm.name" :placeholder="$t('message.messageName')" clearable></el-input>
				</el-form-item>
				<el-form-item>
					<el-button @click="getDataList()">{{ $t('message.search') }}</el-button>
				</el-form-item>
				<el-form-item>
					<el-button type="primary" @click="addOrUpdateHandle()">{{ $t('message.add') }}</el-button>
				</el-form-item>
			</el-form>
			<el-table v-loading="dataListLoading" :data="dataList" border @selection-change="dataListSelectionChangeHandle" @sort-change="dataListSortChangeHandle" style="width: 100%;">
				<el-table-column prop="messageName" :label="$t('message.messageName')" header-align="center" align="center"></el-table-column>
				<el-table-column prop="accepter" :label="$t('message.accepter')" header-align="center" align="center"></el-table-column>
				<el-table-column prop="createDate" :label="$t('message.sendTime')" header-align="center" align="center" width="180"></el-table-column>
				<el-table-column prop="status" :label="$t('message.status')" header-align="center" align="center" width="180"></el-table-column>
				<el-table-column :label="$t('handle')" header-align="center" align="center" width="150">
					<template slot-scope="scope">
						<el-button type="text" size="small" @click="addOrUpdateHandle(scope.row.id)">{{ $t('message.detail') }}</el-button>
						<el-button type="text" size="small" @click="deleteHandle(scope.row.id)">{{ $t('message.delete') }}</el-button>
					</template>
				</el-table-column>
			</el-table>
			<el-pagination :current-page="page" :page-sizes="[10, 20, 50, 100]" :page-size="limit" :total="total" layout="total, sizes, prev, pager, next, jumper" @size-change="pageSizeChangeHandle" @current-change="pageCurrentChangeHandle"> </el-pagination>
			<!-- 弹窗, 新增 / 修改 -->
			<add-or-update v-if="addOrUpdateVisible" ref="addOrUpdate" @refreshDataList="getDataList"></add-or-update>
		</div>
	</el-card>
</template>

<script>
import mixinViewModule from '@/mixins/view-module'
import AddOrUpdate from './message-add-or-update'
export default {
	mixins: [mixinViewModule],
	data() {
		return {
			mixinViewModuleOptions: {
				getDataListURL: '/sys/message/page',
				getDataListIsPage: true,
				deleteURL: '/sys/message',
				deleteIsBatch: true,
			},
			dataForm: {
				name: '',
			},
			dataList: [
				{id: 1, messageName: '活动', accepter: '患者及家属', createDate: '2020-12-24', status: '未读', message: '123', sendPeople: '2', sendMsg: '消息內容' },
				{id: 2, messageName: '通知', accepter: '患者', createDate: '2020-12-24', status: '已读', message: '123', sendPeople: '2', sendMsg: '消息內容' },
			],
		}
	},
	components: {
		AddOrUpdate,
	},
}
</script>

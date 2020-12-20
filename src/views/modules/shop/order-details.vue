<template>
	<el-card shadow="never" class="aui-card--fill">
		<div class="title">基本信息</div>
		<el-divider></el-divider>
		<el-table v-loading="dataListLoading" :data="baseData" border style="width: 100%">
			<el-table-column prop="orderNum" label="订单编号" header-align="center" align="center"></el-table-column>
			<el-table-column prop="userID" label="用户ID" header-align="center" align="center"></el-table-column>
			<el-table-column prop="name" label="姓名" header-align="center" align="center"></el-table-column>
			<el-table-column label="订单金额" header-align="center" align="center">
				<template slot-scope="scope">
					<div>
						<span>订单总额：</span>
						<span>{{ scope.row.amount.totalPrice | formatPrice }}</span>
					</div>
					<div>
						<span>运费金额：</span>
						<span> + {{ scope.row.amount.payPrice | formatPrice }}</span>
					</div>
					<div>
						<span>实付金额：</span>
						<span style="color: red">{{ scope.row.amount.realPrice | formatPrice }}</span>
					</div>
				</template>
			</el-table-column>
			<el-table-column prop="payType" label="支付方式" header-align="center" align="center">
				<template slot-scope="scope">
					<el-tag v-if="scope.row.status === 1">微信支付</el-tag>
					<el-tag v-else>支付宝支付</el-tag>
				</template>
			</el-table-column>
			<el-table-column label="订单状态" header-align="center" align="center">
				<template slot-scope="scope">
					{{ scope.row.status === 1 ? '已完成' : '未完成' }}
				</template>
			</el-table-column>
		</el-table>
		<div class="title">商品信息</div>
		<el-divider></el-divider>
		<el-table v-loading="dataListLoading" :data="goodsData.list" border style="width: 100%">
			<el-table-column label="商品图片" header-align="center" align="center">
				<template>
					<div class="imgCont"></div>
				</template>
			</el-table-column>
			<el-table-column prop="goodsName" label="商品名称" header-align="center" align="center"></el-table-column>
			<el-table-column prop="goodsType" label="商品类型" header-align="center" align="center"></el-table-column>
			<el-table-column label="单价" header-align="center" align="center">
				<template slot-scope="scope">
					{{ scope.row.price | formatPrice }}
				</template>
			</el-table-column>
			<el-table-column label="押金" header-align="center" align="center">
				<template slot-scope="scope">
					{{ scope.row.cashPladge | formatPrice }}
				</template>
			</el-table-column>
			<el-table-column prop="count" label="购买数量" header-align="center" align="center"></el-table-column>
			<el-table-column label="商品总价" header-align="center" align="center">
				<template slot-scope="scope">
					{{ scope.row.totalPrice | formatPrice }}
				</template>
			</el-table-column>
		</el-table>
		<div class="sumCont">
			<div>
				<span>买家留言：</span>
				<span>{{ goodsData.buyMsg || '无' }}</span>
			</div>
			<div>
				<span>总计金额：</span>
				<span>{{ goodsData.totalPrice }}</span>
			</div>
		</div>
		<div class="title">收获信息</div>
		<el-divider></el-divider>
		<el-table v-loading="dataListLoading" :data="getGoodsData" border style="width: 100%">
			<el-table-column prop="name" label="收货人" header-align="center" align="center" width="200"></el-table-column>
			<el-table-column prop="tel" label="收货电话" header-align="center" align="center" width="200"></el-table-column>
			<el-table-column prop="addr" label="收货地址" header-align="center" align="center"></el-table-column>
		</el-table>
		<div class="title">付款信息</div>
		<el-divider></el-divider>
		<el-table v-loading="dataListLoading" :data="payMoneyData" border style="width: 100%">
			<el-table-column label="应付金额" header-align="center" align="center">
				<template slot-scope="scope">
					{{ scope.row.needPayMoney | formatPrice }}
				</template>
			</el-table-column>
			<el-table-column prop="payType" label="支付方式" header-align="center" align="center"></el-table-column>
			<el-table-column label="付款状态" header-align="center" align="center">
				<template slot-scope="scope">
					<el-tag v-if="scope.row.payStatus === 1">已付款</el-tag>
					<el-tag v-else>未付款</el-tag>
				</template>
			</el-table-column>
			<el-table-column prop="payTime" label="付款时间" header-align="center" align="center"></el-table-column>
		</el-table>
		<div class="title">发货信息</div>
		<el-divider></el-divider>
		<el-table v-loading="dataListLoading" :data="sendGoodsData" border style="width: 100%">
			<el-table-column label="发货状态" header-align="center" align="center">
				<template slot-scope="scope">
					<el-tag v-if="scope.row.sendStatus === 1">已发货</el-tag>
					<el-tag v-else>未发货</el-tag>
				</template>
			</el-table-column>
			<el-table-column prop="sendTime" label="发货时间" header-align="center" align="center"></el-table-column>
		</el-table>
	</el-card>
</template>

<script>
export default {
	data() {
		return {
			baseData: [{ orderNum: 202003264479623, userID: 1001, name: '张三', amount: { totalPrice: 4000, payPrice: 50, realPrice: 4500 }, payType: '微信支付', status: 1 }],
			goodsData: {
				buyMsg: '',
				list: [
					{ img: '', goodsName: '中控设备租赁3个月', goodsType: '设备租赁', price: 500, cashPladge: 500, count: 2, totalPrice: 1000 },
					{ img: '', goodsName: '上颖运动模块', goodsType: '设备购买', price: 1000, cashPladge: 0, count: 2, totalPrice: 2000 },
				],
				totalPrice: 4000,
			},
			getGoodsData: [{ name: '张三', tel: '15035555981', addr: '北京市丰台区四环西路19号' }],
			payMoneyData: [{ needPayMoney: 4050, payType: '微信支付', payStatus: 1, payTime: '2020-10-20 19:43:32' }],
			sendGoodsData: [{ sendStatus: 1, sendTime: '2020-12-23 23:43:12' }],
		}
	},
	methods: {
		retu: function() {
			this.$router.go(-1)
		},
	},

	filters: {
		formatPrice(price) {
			return '￥ ' + price.toFixed(2)
		},
	},
}
</script>
<style scoped>
.btn {
	margin-top: 20px !important;
}
.title {
	padding-top: 20px;
	margin-bottom: -10px;
}
.imgCont {
	width: 40px;
	height: 40px;
	background-color: #ccc;
}
.sumCont {
	display: flex;
	justify-content: space-between;
	align-items: center;
	padding: 20px;
}
</style>

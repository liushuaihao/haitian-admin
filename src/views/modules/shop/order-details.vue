<template>
  <div>
    <el-card
      v-loading="loading"
      shadow="never"
      class="aui-card--fill"
      v-if="this.dataForm"
    >
      <div class="title">基本信息</div>
      <el-divider></el-divider>
      <el-table :data="baseData" border style="width: 100%">
        <el-table-column
          prop="id"
          label="订单编号"
          header-align="center"
          align="center"
        ></el-table-column>
        <el-table-column
          prop="userId"
          label="用户ID"
          header-align="center"
          align="center"
        ></el-table-column>
        <el-table-column
          prop="realName"
          label="姓名"
          header-align="center"
          align="center"
        ></el-table-column>
        <el-table-column label="订单金额" header-align="center" align="center">
          <template slot-scope="scope">
            <div>
              <span>订单总额：</span>
              <span>{{ scope.row.amount | formatPrice }}</span>
            </div>
            <div v-if="scope.row.deposit">
              <span>押金金额：</span>
              <span>{{ scope.row.deposit | formatPrice }}</span>
            </div>

            <div v-if="scope.row.transit">
              <span>运费金额：</span>
              <span> + {{ scope.row.transit | formatPrice }}</span>
            </div>
            <div v-if="status != 0 && status != 3">
              <span>实付金额：</span>
              <span style="color: red">{{
                scope.row.amountTotal | formatPrice
              }}</span>
            </div>
          </template>
        </el-table-column>
        <el-table-column
          prop="payChannel"
          label="支付方式"
          header-align="center"
          align="center"
          v-if="status != 0 && status != 3"
        >
          <template slot-scope="scope">
            <el-tag v-if="scope.row.payChannel == 0" type="success"
              >微信支付</el-tag
            >
            <el-tag v-if="scope.row.payChannel == 1">支付宝支付</el-tag>
          </template>
        </el-table-column>
        <el-table-column label="订单状态" header-align="center" align="center">
          <template slot-scope="scope">
            <el-tag
              :type="
                scope.row.orderStatus == 2
                  ? 'success'
                  : scope.row.orderStatus == 3
                  ? 'danger'
                  : ''
              "
            >
              {{ orderStatus[scope.row.orderStatus] }}</el-tag
            >
          </template>
        </el-table-column>
      </el-table>
      <div class="title">商品信息</div>
      <el-divider></el-divider>
      <el-table
        :data="goodsData.goodsList"
        v-if="goodsData.goodsList.length"
        border
        style="width: 100%"
      >
        <el-table-column label="商品图片" header-align="center" align="center">
          <template slot-scope="scope">
            <div class="imgCont">
              <img :src="scope.row.picUrl" alt="" />
            </div>
          </template>
        </el-table-column>
        <el-table-column
          prop="goodsName"
          label="商品名称"
          header-align="center"
          align="center"
        ></el-table-column>
        <el-table-column
          prop="goodsType"
          label="商品类型"
          header-align="center"
          align="center"
        >
          <template slot-scope="scope">
            {{ goodsType[scope.row.goodsType] }}
          </template>
        </el-table-column>
        <el-table-column label="单价" header-align="center" align="center">
          <template slot-scope="scope">
            <!-- {{ scope.row.pkgPrice }} -->
            {{ scope.row.pkgPrice | formatPrice }}
          </template>
        </el-table-column>
        <el-table-column label="押金" header-align="center" align="center">
          <template slot-scope="scope" v-if="scope.row.deposit">
            {{ scope.row.deposit | formatPrice }}
          </template>
        </el-table-column>
        <el-table-column
          prop="num"
          label="购买数量"
          header-align="center"
          align="center"
        ></el-table-column>
        <el-table-column label="商品总价" header-align="center" align="center">
          <template slot-scope="scope">
            {{ scope.row.amount | formatPrice }}
          </template>
        </el-table-column>
      </el-table>
      <div class="sumCont">
        <div>
          <span></span>
          <span></span>
        </div>
        <div>
          <span>总计金额：</span>
          <span>{{ goodsData.amountTotal | formatPrice }}</span>
        </div>
      </div>
      <div class="title">收货人信息</div>
      <el-divider></el-divider>
      <el-table :data="getGoodsData" border style="width: 100%">
        <el-table-column
          prop="receiverName"
          label="收货人"
          header-align="center"
          align="center"
          width="200"
        ></el-table-column>
        <el-table-column
          prop="receiverMobile"
          label="收货电话"
          header-align="center"
          align="center"
          width="200"
        ></el-table-column>
        <el-table-column
          prop="receiverAddress"
          label="收货地址"
          header-align="center"
          align="center"
        ></el-table-column>
      </el-table>
      <template v-if="status != 0 && status != 3">
        <div>
          <div class="title">付款信息</div>
          <el-divider></el-divider>
          <el-table :data="payMoneyData" border style="width: 100%">
            <el-table-column
              label="应付金额"
              header-align="center"
              align="center"
            >
              <template slot-scope="scope">
                {{ scope.row.amountTotal | formatPrice }}
              </template>
            </el-table-column>
            <el-table-column
              prop="payChannel"
              label="支付方式"
              header-align="center"
              align="center"
            >
              <template slot-scope="scope">
                <el-tag v-if="scope.row.payChannel == 0" type="success"
                  >微信支付</el-tag
                >
                <el-tag v-if="scope.row.payChannel == 1">支付宝支付</el-tag>
              </template></el-table-column
            >
            <el-table-column
              label="付款状态"
              header-align="center"
              align="center"
            >
              <template slot-scope="scope">
                <el-tag v-if="scope.row.payStatus == 1" type="success"
                  >已付款</el-tag
                >
                <el-tag v-else>未付款</el-tag>
              </template>
            </el-table-column>
            <el-table-column
              prop="payTime"
              label="付款时间"
              header-align="center"
              align="center"
            ></el-table-column>
          </el-table>
        </div>
      </template>
      <template>
        <div v-if="status == 2">
          <div class="title">发货信息</div>
          <el-divider></el-divider>
          <el-table :data="sendGoodsData" border style="width: 100%">
            <el-table-column
              label="发货状态"
              header-align="center"
              align="center"
            >
              <template slot-scope="scope">
                <el-tag type="success" v-if="scope.row.transStatus == 1"
                  >已发货</el-tag
                >
                <el-tag v-else>未发货</el-tag>
              </template>
            </el-table-column>
            <el-table-column
              prop="transTime"
              label="发货时间"
              header-align="center"
              align="center"
            ></el-table-column>
          </el-table>
        </div>
      </template>
    </el-card>
  </div>
</template>

<script>
export default {
  data() {
    return {
      loading: true,
      goodsType: {
        0: "在线服务",
        1: "设备租赁",
        2: "设备购买",
      },
      status: "0",
      orderStatus: {
        0: "已下单",
        1: "已付款",
        2: "已完成",
        3: "已取消",
      },
      baseData: [],
      goodsData: {
        goodsList: [],
        amountTotal: "",
      },
      getGoodsData: [],
      payMoneyData: [],
      sendGoodsData: [],
      dataForm: {
        id: "",
        goodsList: [],
      },
    };
  },
  created() {
    this.dataForm.id = this.$route.params.id;
    this.init();
  },
  methods: {
    init() {
      this.$nextTick(() => {
        this.getInfo();
      });
    },
    // 获取信息
    getInfo() {
      this.$http
        .get(`/shop/order/getOrderDetail?id=${this.dataForm.id}`)
        .then(({ data: res }) => {
          if (res.code != 0) {
            return this.$message.error(res.msg);
          }
          this.dataForm = {
            ...this.dataForm,
            ...res.data,
          };
          this.status = res.data.orderStatus;
          // 基本信息
          this.$set(this.baseData, 0, {
            id: res.data.id,
            userId: res.data.userId,
            realName: res.data.realName,
            amount: res.data.amount,
            deposit: res.data.deposit,
            transit: res.data.transit,
            amountTotal: res.data.amountTotal,
            payChannel: res.data.payChannel,
            orderStatus: res.data.orderStatus,
          });
          // 商品信息
          this.$set(this.goodsData, "goodsList", res.data.goodsList);
          this.$set(this.goodsData, "amountTotal", res.data.amountTotal);
          // 收货信息
          this.$set(this.getGoodsData, 0, {
            receiverAddress: res.data.receiverAddress, //收货地址
            receiverMobile: res.data.receiverMobile, //收货电话
            receiverName: res.data.receiverName, //收货人
          });
          // 付款信息
          this.$set(this.payMoneyData, 0, {
            amountTotal: res.data.amountTotal, // 应付金额
            payChannel: res.data.payChannel, //支付方式 0微信 1支付宝	string
            payStatus: res.data.payStatus, //支付状态0 未支付 1已支付	string
            payTime: res.data.payTime, //付款时间	string
          });
          //发货信息
          this.$set(this.sendGoodsData, 0, {
            transStatus: res.data.transStatus, //发货状态 0未发货 1已发货	string
            transTime: res.data.transTime, //发货时间
          });
          this.loading = false;
        })
        .catch(() => {});
    },
  },

  filters: {
    formatPrice(price) {
      if (price) {
        return "￥ " + price.toFixed(2);
      }
    },
  },
};
</script>
<style scoped lang="scss">
.btn {
  margin-top: 20px !important;
}
.title {
  padding-top: 20px;
  margin-bottom: -10px;
}
.imgCont {
  img {
    width: 80px;
    height: 80px;
  }
}
.sumCont {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 20px;
}
</style>

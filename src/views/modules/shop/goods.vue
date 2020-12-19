<template>
  <el-card shadow="never" class="aui-card--fill">
    <div class="mod-essential_aoe">
      <el-form :inline="true" :model="commodity" @keyup.enter.native="getDataList()">
        <!-- $hasPermission('shop:goods:save') 按钮权限配置 'shop:goods:save' 标识 -->
        <el-form-item>
          <el-button
            v-if="$hasPermission('shop:goods:save')"
            type="primary"
            @click="addOrUpdateHandle()"
          >{{ $t("add") }}</el-button>
        </el-form-item>
        <el-form-item>
          <el-input v-model="commodity.id" placeholder="商品ID" clearable></el-input>
        </el-form-item>
        <el-form-item>
          <el-input v-model="commodity.name" placeholder="商品名称" clearable></el-input>
        </el-form-item>
        <el-form-item>
          <el-select class="selects" clearable v-model="commodity.type" placeholder="商品类型">
            <el-option
              v-for="item in commodityType"
              :key="item.value"
              :label="item.label"
              :value="item.value"
            ></el-option>
          </el-select>
        </el-form-item>
        <el-form-item>
          <el-button @click="getDataList()">{{ $t('query') }}</el-button>
        </el-form-item>
      </el-form>
      <el-table
        v-loading="dataListLoading"
        :data="dataList"
        border
        @selection-change="dataListSelectionChangeHandle"
        @sort-change="dataListSortChangeHandle"
        style="width: 100%;"
      >
      <el-table-column prop="id" label="商品ID" header-align="center" align="center"></el-table-column>
        <el-table-column prop="image" label="图片" header-align="center" align="center">
          <template slot-scope="scope">
            <div>
              <img :src="scope.row.image" width="50px" height="50px" alt />
            </div>
          </template>
        </el-table-column>
        <el-table-column prop="goodsName" label="商品名称" header-align="center" align="center"></el-table-column>
        <el-table-column prop="goodsType" label="商品类型" header-align="center" align="center"></el-table-column>
        <el-table-column prop="number" label="库存" header-align="center" align="center"></el-table-column>
        <el-table-column
          :label="$t('handle')"
          fixed="right"
          header-align="center"
          align="center"
          width="150"
        >
          <template > <!-- slot-scope="scope" -->
            <!-- <el-button type="text" size="small" @click="forwardUrl(scope.row)">详情</el-button> -->
            <el-button type="text" size="small">修改</el-button>
            <el-button type="text" size="small">删除</el-button>
          </template>
        </el-table-column>
      </el-table>
      <el-pagination
        :current-page="page"
        :page-sizes="[10, 20, 50, 100]"
        :page-size="limit"
        :total="total"
        layout="total, sizes, prev, pager, next, jumper"
        @size-change="pageSizeChangeHandle"
        @current-change="pageCurrentChangeHandle"
      ></el-pagination>

      <!-- 弹窗, 新增 / 修改 -->
      <add-or-update v-if="addOrUpdateVisible" ref="addOrUpdate" @refreshDataList="getDataList"></add-or-update>
    </div>
  </el-card>
</template>

<script>
import mixinViewModule from "@/mixins/view-module";
import AddOrUpdate from "./goods-add-or-update.vue";
// import { addDynamicRoute } from "@/router"; // 添加动态路由
export default {
  components: {
    AddOrUpdate
  },
  mixins: [mixinViewModule],
  data () {
    return {
      mixinViewModuleOptions: {
        getDataListURL: "",
        getDataListIsPage: true,
        deleteURL: "",
        deleteIsBatch: true
      },
      dataList: [
        {
          id: "1",
          image:
            "https://ss0.bdstatic.com/70cFvHSh_Q1YnxGkpoWK1HF6hhy/it/u=2536716619,1370324927&fm=26&gp=0.jpg",
          goodsName: "2020夏季新款女装韩版",
          goodsType: "在线服务",
          number: "100",
          type: 1
        },
        {
          id: "2",
          image:
            "https://ss0.bdstatic.com/70cFvHSh_Q1YnxGkpoWK1HF6hhy/it/u=2536716619,1370324927&fm=26&gp=0.jpg",
          goodsName: "2020夏季新款女装韩版",
          goodsType: "设备购买",
          number: "100",
          type: 2
        },
        {
          id: "3",
          image:
            "https://ss0.bdstatic.com/70cFvHSh_Q1YnxGkpoWK1HF6hhy/it/u=2536716619,1370324927&fm=26&gp=0.jpg",
          goodsName: "2020夏季新款女装韩版",
          goodsType: "设备租赁",
          number: "100",
          type: 3
        }
      ],
      dataForm: {
        identityCard: "",
        name: "",
        mobile: "",
        id: ""
      },
      commodity: {
        id: "",
        name: "",
        type: "",
      },
      commodityType: [
        {
          value: "0",
          label: "在线服务"
        },
        {
          value: "1",
          label: "设备租赁"
        },
        {
          value: "2",
          label: "设备购买"
        }
      ],
    };
  },
  methods: {
    // forwardUrl (row) {
    //   let type = row.type;
    //   var routeParams = {
    //     routeName: `${this.$route.name}__instance_${row.id}`,
    //     menuId: `${this.$route.meta.menuId}`,
    //     title: `${this.$route.meta.title}详情 - ${row.goodsName}`,
    //     path: "shop/goods-details",
    //     params: { type }
    //   };
    //   addDynamicRoute(routeParams, this.$router, this.$route);
    // }
  }
};
</script>

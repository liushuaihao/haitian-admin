<template>
  <el-card shadow="never" class="aui-card--fill">
    <div class="mod-essential_aoe">
      <el-form
        :inline="true"
        :model="dataForm"
        @keyup.enter.native="getDataList()"
      >
        <!-- $hasPermission('shop:goods:save') 按钮权限配置 'shop:goods:save' 标识 -->
        <el-form-item>
          <el-input
            v-model="dataForm.goodsId"
            placeholder="商品ID"
            clearable
          ></el-input>
        </el-form-item>
        <el-form-item>
          <el-input
            v-model="dataForm.goodsName"
            placeholder="商品名称"
            clearable
          ></el-input>
        </el-form-item>
        <el-form-item>
          <el-select
            class="selects"
            clearable
            v-model="dataForm.goodsType"
            placeholder="商品类型"
          >
            <el-option
              v-for="item in type"
              :key="item.value"
              :label="item.label"
              :value="item.value"
            ></el-option>
          </el-select>
        </el-form-item>
        <el-form-item>
          <el-button @click="getDataList()">{{ $t("query") }}</el-button>
        </el-form-item>
        <el-form-item>
          <el-button
            v-if="$hasPermission('shop:goods:save')"
            type="primary"
            @click="addGoods()"
            >添加商品</el-button
          >
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
        <el-table-column
          prop="id"
          label="商品ID"
          header-align="center"
          align="center"
        ></el-table-column>
        <el-table-column
          prop="picUrl"
          label="商品图片"
          header-align="center"
          align="center"
        >
          <template slot-scope="scope">
            <div>
              <img :src="scope.row.picUrl[0]" width="50px" height="50px" alt />
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
            <div>
              {{
                scope.row.goodsType == 0
                  ? "在线服务"
                  : scope.row.goodsType == 1
                  ? "设备租赁"
                  : scope.row.goodsType == 2
                  ? "设备购买"
                  : ""
              }}
            </div>
          </template>
        </el-table-column>
        <el-table-column
          prop="stock"
          label="库存"
          header-align="center"
          align="center"
        ></el-table-column>
        <el-table-column
          :label="$t('handle')"
          fixed="right"
          header-align="center"
          align="center"
          width="150"
        >
          <template slot-scope="scope">
            <!-- slot-scope="scope" -->
            <!-- <el-button type="text" size="small" @click="forwardUrl(scope.row)">详情</el-button> -->
            <el-button
              type="text"
              v-if="$hasPermission('shop:goods:update')"
              @click="addGoods(scope.row.id)"
              size="small"
              >修改</el-button
            >
            <el-button
              type="text"
              v-if="$hasPermission('shop:goods:delete')"
              @click="deleteHandle(scope.row.id)"
              size="small"
              >删除</el-button
            >
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
      <add-or-update
        v-if="addOrUpdateVisible"
        ref="addOrUpdate"
        @refreshDataList="getDataList"
      ></add-or-update>
    </div>
  </el-card>
</template>

<script>
import mixinViewModule from "@/mixins/view-module";
import AddOrUpdate from "./goods-add-or-update.vue";
import { addDynamicRoute } from "@/router"; // 添加动态路由
export default {
  components: {
    AddOrUpdate,
  },
  mixins: [mixinViewModule],
  data() {
    return {
      mixinViewModuleOptions: {
        getDataListURL: "/shop/goods/list",
        getDataListIsPage: true,
        activatedIsNeed: true,
        deleteURL: "/shop/goods/delete",
        deleteIsBatch: true,
      },
      type: [
        {
          value: "0",
          label: "在线服务",
        },
        {
          value: "1",
          label: "设备租赁",
        },
        {
          value: "2",
          label: "设备购买",
        },
      ],
    };
  },
  methods: {
    addGoods(id) {
      var routeParams = {
        routeName: `${this.$route.name}__instance_${id ? id : "add"}`,
        menuId: `${this.$route.meta.menuId}`,
        title: `${id ? "修改商品" : "添加商品"}`,
        path: "shop/goods-add-or-update",
        params: { id: id },
      };
      addDynamicRoute(routeParams, this.$router, this.$route);
    },
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
  },
};
</script>

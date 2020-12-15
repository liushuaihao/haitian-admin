<template>
  <el-card shadow="never" class="aui-card--fill">
    <div class="mod-essential_aoe">
      <el-pagination
        :current-page="page"
        :page-sizes="[10, 20, 50, 100]"
        :page-size="limit"
        :total="total"
        layout="total, sizes, prev, pager, next, jumper"
        @size-change="pageSizeChangeHandle"
        @current-change="pageCurrentChangeHandle"
      ></el-pagination>
    </div>
  </el-card>
</template>

<script>
import mixinViewModule from "@/mixins/view-module";
import { addDynamicRoute } from "@/router"; // 添加动态路由
export default {
  mixins: [mixinViewModule],
  data () {
    return {
      mixinViewModuleOptions: {
        getDataListURL: "",
        getDataListIsPage: true,
        deleteURL: "",
        deleteIsBatch: true
      },
      dataForm: {
        identityCard: "",
        name: "",
        mobile: "",
        id: ""
      },
      dataList: [
        {
          id: "1111-0000-1111",
          name: "张三",
          mobile: "13012345671",
          identityCard: "11011011123456789",
          heart: "60", // 心率值
          tmti: "2019-04-05" // 最近时间
        },
        {
          id: "1111-0000-1111",
          name: "张三",
          mobile: "13012345671",
          identityCard: "11011011123456789",
          heart: "70",
          tmti: "2019-04-05"
        },
        {
          id: "1111-0000-1111",
          name: "张三",
          mobile: "13012345671",
          identityCard: "11011011123456789",
          heart: "60",
          tmti: "2019-04-05"
        },
        {
          id: "1111-0000-1111",
          name: "张三",
          mobile: "13012345671",
          identityCard: "11011011123456789",
          heart: "80",
          tmti: "2019-04-05"
        },
        {
          id: "1111-0000-1111",
          name: "张三",
          mobile: "13012345671",
          identityCard: "11011011123456789",
          heart: "50",
          tmti: "2019-04-05"
        },
      ]
    };
  },
  methods: {
    forwardUrl (row) {
      var routeParams = {
        routeName: `${this.$route.name}__instance_${row.id}`,
        menuId: `${this.$route.meta.menuId}`,
        title: `${this.$route.meta.title}详情 - ${row.name}`,
        path: "essential/aoe-details",
        params: {}
      };
      addDynamicRoute(routeParams, this.$router, this.$route);
    }
  }
};
</script>

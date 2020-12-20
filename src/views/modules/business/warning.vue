<template>
  <!-- 预警设置 -->
  <el-card shadow="never" class="aui-card--fill">
    <div class="mod-mod-essential_sls">
      <el-form :inline="true" :model="dataForm" @keyup.enter.native="getDataList()">
        <el-form-item>
          <el-input
            v-model="dataForm.identityCard"
            :placeholder="$t('essential.identityCard')"
            clearable
          ></el-input>
        </el-form-item>
        <el-form-item>
          <el-input v-model="dataForm.name" :placeholder="$t('essential.name')" clearable></el-input>
        </el-form-item>
        <el-form-item>
          <el-input v-model="dataForm.mobile" :placeholder="$t('essential.mobile')" clearable></el-input>
        </el-form-item>
        <el-form-item>
          <el-input v-model="dataForm.id" :placeholder="$t('essential.id')" clearable></el-input>
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
        <el-table-column
          prop="name"
          :label="$t('essential.name')"
          header-align="center"
          align="center"
        ></el-table-column>
        <el-table-column
          prop="mobile"
          :label="$t('essential.mobile')"
          header-align="center"
          align="center"
        ></el-table-column>
        <el-table-column prop="step" label="步数" header-align="center" align="center"></el-table-column>
        <el-table-column prop="respiratory" label="呼吸率" header-align="center" align="center"></el-table-column>
        <el-table-column prop="distance" label="距离" header-align="center" align="center"></el-table-column>
        <el-table-column prop="minheartL" label="心率" header-align="center" align="center"></el-table-column>
        <el-table-column prop="momental" label="运动量" header-align="center" align="center"></el-table-column>

        <el-table-column
          :label="$t('handle')"
          fixed="right"
          header-align="center"
          align="center"
          width="150"
        >
          <template slot-scope="scope">
            <el-button  type="text" size="small" @click="editWarn(scope.row)" >{{ $t('update') }}</el-button>
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
    </div>
    <warning-edit ref="warningUpdateDialog"> </warning-edit>
  </el-card>
</template>

<script>
import mixinViewModule from "@/mixins/view-module";
import warningEdit from './warning-edit'
export default {
  mixins: [mixinViewModule],
  components: { warningEdit },
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
          name: "张三",
          mobile: "13012345671",
          step: "100-1000",
          respiratory: "10-100",
          distance: "100-1000",
          minheartL: "60-120",
          momental: "500-5000",
          id: 10
        },
        {
          name: "张三",
          mobile: "13012345671",
          step: "100-1000",
          respiratory: "10-100",
          distance: "100-1000",
          minheartL: "60-120",
          momental: "500-5000",
          id: 10
        },
        {
          name: "张三",
          mobile: "13012345671",
          step: "100-1000",
          respiratory: "10-100",
          distance: "100-1000",
          minheartL: "60-120",
          momental: "500-5000",
          id: 10
        },
        {
          name: "张三",
          mobile: "13012345671",
          step: "100-1000",
          respiratory: "10-100",
          distance: "100-1000",
          minheartL: "60-120",
          momental: "500-5000",
          id: 10
        },
        {
          name: "张三",
          mobile: "13012345671",
          step: "100-1000",
          respiratory: "10-100",
          distance: "100-1000",
          minheartL: "60-120",
          momental: "500-5000",
          id: 10
        }
      ]
    };
  },
  methods: {
    editWarn (row) {
      this.$refs.warningUpdateDialog.visible = true
    }
  }
};
</script>

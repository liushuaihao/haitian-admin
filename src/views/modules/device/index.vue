<template>
  <el-card shadow="never" class="aui-card--fill">
    <div class="mod-sys__dept">
      <el-form :inline="true" :model="dataForm" @keyup.enter.native="getDataList()">
        <!-- 用户身份证/姓名/电话号/中控设备id号/测量设备ID号 -->
        <el-form-item>
            <el-input placeholder="用户身份证" v-model="dataForm.idCard"></el-input>
        </el-form-item>
        <el-form-item>
            <el-input placeholder="姓名" v-model="dataForm.name"></el-input>
        </el-form-item>
        <el-form-item>
            <el-input placeholder="电话号" v-model="dataForm.mobile"></el-input>
        </el-form-item>
        <el-form-item>
            <el-input placeholder="中控设备ID号" v-model="dataForm.centerId"></el-input>
        </el-form-item>
        <el-form-item>
            <el-input placeholder="测量设备ID号" v-model="dataForm.measureId"></el-input>
        </el-form-item>
        <el-form-item>
            <el-select v-model="dataForm.bind" placeholder="绑定状态">
                <el-option label="全部" value="quanbu"></el-option>
                <el-option label="绑定" value="bangding"></el-option>
                <el-option label="空闲" value="kongxian"></el-option>
            </el-select>
        </el-form-item>
        <el-form-item>
            <el-input placeholder="位置属性" v-model="dataForm.position"></el-input>
        </el-form-item>
        <el-form-item>
            <el-select v-model="dataForm.measure" placeholder="测量设备">
                <el-option label="全部" value="quanbu"></el-option>
                <el-option label="上肢设备" value="shangzhi"></el-option>
                <el-option label="下肢设备" value="xiazhi"></el-option>
                <el-option label="呼吸设备" value="huxi"></el-option>
                <el-option label="心电设备" value="xindian"></el-option>
                <el-option label="肌电设备" value="jidian"></el-option>
            </el-select>
        </el-form-item>
        <el-form-item>
          <el-button @click="getDataList()">{{ $t('query') }}</el-button>
          <el-button  type="primary" @click="addOrUpdateHandle()">{{ $t('add') }}</el-button>
        </el-form-item>
      </el-form>
      <!-- 设备ID 姓名 电话 身份证号 状态 时间 -->
      <el-table v-loading="dataListLoading" :data="dataList" row-key="id" border style="width: 100%;">
        <el-table-column prop="id" label="设备ID" header-align="center"></el-table-column>
        <el-table-column prop="name" label="姓名" header-align="center" align="center"></el-table-column>
        <el-table-column prop="mobile" :label="$t('sms.mobile')" header-align="center" align="center" ></el-table-column>
        <el-table-column prop="idCard" label="身份证号" header-align="center" align="center"></el-table-column>
        <el-table-column prop="status" label="状态" header-align="center" align="center"></el-table-column>
        <el-table-column prop="dataTime" label="时间" header-align="center" align="center"></el-table-column>
        <el-table-column :label="$t('handle')" fixed="right" header-align="center" align="center" width="200">
          <template slot-scope="scope">
            <el-button type="text" size="small" @click="infoHandle(scope.row.id)">详情</el-button>
            <el-button type="text" size="small" @click="deleteHandle(scope.row.id)">{{ $t('delete') }}</el-button>
            <el-button type="text" size="small" @click="unbindHandle(scope.row.id)">解绑</el-button>
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

      <!-- 详情 -->
      <add-or-update v-if="addOrUpdateVisible" ref="addOrUpdate" @refreshDataList="getDataList"></add-or-update>
    </div>
  </el-card>
</template>

<script>
import mixinViewModule from '@/mixins/view-module'
import AddOrUpdate from './index-add-or-update'
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
      dataList:[
        {
          id: 10
        }
      ]
    }
  },
  components: {
    AddOrUpdate
  },
  methods: {
    unbindHandle (id) {

    },
    infoHandle (id) {
      this.addOrUpdateVisible = true
      this.$nextTick(() => {
        this.$refs.addOrUpdate.dataForm.id = id
        this.$refs.addOrUpdate.init()
      })
    }
  }
}
</script>

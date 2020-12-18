<template>
  <el-card shadow="never" class="aui-card--fill">
    <el-card>
      <el-button type="success">导出</el-button>
      <h3>基本信息</h3>
      <el-form ref="form" label-width="130px">
        <div class="personal">
          <el-form-item label="姓名：">
            <div>{{basicInformation.name}}</div>
          </el-form-item>
          <el-form-item label="姓别：">
            <div>{{basicInformation.sex}}</div>
          </el-form-item>
          <el-form-item label="年龄：">
            <div>{{basicInformation.age}}</div>
          </el-form-item>
          <el-form-item label="身高：">
            <div>{{basicInformation.stature}}</div>
          </el-form-item>
          <el-form-item label="体重：">
            <div>{{basicInformation.weight}}</div>
          </el-form-item>
        </div>
        <el-form-item label="电话：">
          <div>{{basicInformation.tel}}</div>
        </el-form-item>
        <el-form-item label="住址：">
          <div>{{basicInformation.site}}</div>
        </el-form-item>
        <h4>紧急联系人</h4>
        <el-form-item label="张儿子：">
          <div>16601275207</div>
        </el-form-item>
        <el-form-item label="张女儿：">
          <div>16601275207</div>
        </el-form-item>
        <el-form-item label="张弟弟：">
          <div>16601275207</div>
        </el-form-item>
        <h4>关联设备</h4>
        <el-form-item label="主控设备：">
          <div>1111-0000-1111</div>
        </el-form-item>
        <el-form-item label="手环设备：">
          <div>1111-0000-1111</div>
        </el-form-item>
        <el-form-item label="心电设备：">
          <div>1111-0000-1111</div>
        </el-form-item>
        <el-form-item label="呼吸设备：">
          <div>1111-0000-1111</div>
        </el-form-item>
        <el-form-item label="下肢运动及机电：">
          <div>1111-0000-1111</div>
        </el-form-item>
      </el-form>
    </el-card>
    <el-card>
      <h4>定位信息</h4>
      <el-form>
        <div class="human">
          <el-form-item label="经度：">
            <div>117.087013</div>
          </el-form-item>
          <el-form-item label="纬度：">
            <div>36.200089</div>
          </el-form-item>
        </div>
      </el-form>
      <gpsMap />
    </el-card>
  </el-card>
</template>
<script>
import gpsMap from "./gps-map.vue";
export default {
  components: {
    gpsMap
  },
  data () {
    return {
      basicInformation: {
        name: "张三",
        sex: "男",
        age: "22",
        stature: "165",
        weight: "70",
        tel: "16601275207",
        site: "北京市海淀区中关村科技大厦502"
      }
    };
  },
  mounted () {
    this.map();
  },
  methods: {}
};
</script>
<style scoped>
.details > h3 {
  text-align: center;
  line-height: 80px;
  margin: 0;
}
.personal {
  display: flex;
  width: 70%;
}
.personal > p {
  flex: 1;
}
#container {
  overflow: hidden;
  width: 100%;
  height: 500px;
  margin: 0;
  font-family: "微软雅黑";
}
.human {
  width: 36%;
  display: flex;
}
.human > div {
  flex: 1;
}
</style>

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
    <el-card class="sleep">
      Maecenas eu elit fringilla, sollicitudin magna eget, convallis tellus. Morbi eget elementum magna. Cras lacinia risus id purus malesuada, mattis posuere sapien aliquet. Praesent porttitor lobortis sollicitudin. Sed dolor nisi, tincidunt a nibh et, malesuada molestie sem. Maecenas elementum auctor est. Cras vitae consectetur eros. Aliquam sodales elementum pretium. Nulla non lacus a magna iaculis vulputate nec eu tortor. Nunc commodo lacinia posuere. Sed eu enim ut odio finibus efficitur. Ut sit amet commodo quam, id ullamcorper est. Maecenas aliquet ipsum tortor, sit amet facilisis nisi efficitur in. Integer congue rhoncus feugiat.
    </el-card>
  </el-card>
</template>
<script>
export default {
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
.sleep{
  padding-bottom: 100px;
}
</style>

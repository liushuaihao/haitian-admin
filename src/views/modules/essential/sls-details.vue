<template>
  <el-card shadow="never" class="aui-card--fill">
    <div class="details">
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
        <h4>步频步幅情况</h4>
        <el-form :inline="true"  @keyup.enter.native="getDataList()">
          <el-form-item label="分析时段：">
            <el-select v-model="time" placeholder="分析时段">
              <el-option label="24小时" value="hour"></el-option>
              <el-option label="1周" value="week"></el-option>
              <el-option label="1个月" value="month"></el-option>
            </el-select>
          </el-form-item>
          <div class="human">
        </div>
        <div class="frequency">
        <div>步频展示</div>
        <div>步幅展示</div>
        </div>
        </el-form>
      </el-card>
    </div>
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
      },
      daterange: "",
      time: ""
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
.tjx {
  height: 300px;
  display: flex;
  justify-content: center;
  align-items: center;
  margin-bottom: 20px;
}
.human{
  width: 36%;
  display: flex;
}
.human>div{
  flex: 1;
}
.frequency{
  display: flex;
}
.frequency>div{
  flex: 1;
  height: 300px;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: #ccc;
  margin: 0 10px;
}
</style>

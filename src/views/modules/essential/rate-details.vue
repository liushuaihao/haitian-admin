<template>
  <div class="details">
    <el-button type="success">导出</el-button>
    <h3>基本信息</h3>
    <el-card>
      <div class="personal">
        <p>
          姓名：
          <span>{{basicInformation.name}}</span>
        </p>
        <p>
          性别：
          <span>{{basicInformation.sex}}</span>
        </p>
        <p>
          年龄：
          <span>{{basicInformation.age}}</span>
        </p>
        <p>
          身高：
          <span>{{basicInformation.stature}}cm</span>
        </p>
        <p>
          体重：
          <span>{{basicInformation.weight}}kg</span>
        </p>
      </div>
      <div>
        <p>
          电话：
          <span>{{basicInformation.tel}}</span>
        </p>
      </div>
      <div>
        <p>
          住址：
          <span>{{basicInformation.site}}</span>
        </p>
      </div>
      <h4>紧急联系人</h4>
      <div>
        <p>
          张儿子：
          <span>16601275207</span>
        </p>
      </div>
      <div>
        <p>
          张女儿：
          <span>16601275207</span>
        </p>
      </div>
      <div>
        <p>
          张弟弟：
          <span>16601275207</span>
        </p>
      </div>
      <h4>关联设备</h4>
      <div>
        <p>
          主控设备：
          <span>1111-0000-1111</span>
        </p>
      </div>
      <div>
        <p>
          手环设备：
          <span>1111-0000-1111</span>
        </p>
      </div>
      <div>
        <p>
          心电设备：
          <span>1111-0000-1111</span>
        </p>
      </div>
      <div>
        <p>
          呼吸设备：
          <span>1111-0000-1111</span>
        </p>
      </div>
      <div>
        <p>
          下肢运动及机电：
          <span>1111-0000-1111</span>
        </p>
      </div>
    </el-card>
  </div>
</template>
<script>
export default {
  data () {
    return {
      basicInformation: {
        name: "张三",
        sex: "男",
        age: "66",
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
  width: 39%;
}
.personal > p {
  flex: 1;
}
</style>

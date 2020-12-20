<template>
  <el-card shadow="never" class="aui-card--fill">
    <p>
     <el-button
        type="success"
        v-if="$hasPermission('business:report:edit')"
      >修改</el-button>
    <el-button type="success" v-if="$hasPermission('business:report:expor')">导出</el-button>
    </p>
    <div class="details-box">
      <div class="details-title">
          基本信息
      </div>
      <div class="details-main">
        <p>
         <el-row :gutter="10">
          <el-col :span="4">姓名：{{basicInformation.name}}</el-col>
          <el-col :span="4">姓别：{{basicInformation.sex}}</el-col>
          <el-col :span="4">年龄：{{basicInformation.age}}</el-col>
          <el-col :span="4">身高：{{basicInformation.stature}}cm</el-col>
          <el-col :span="4">体重：{{basicInformation.weight}}kg</el-col>
         </el-row>
        </p>
        <p>电话：{{basicInformation.tel}}</p>
        <p>住址：{{basicInformation.site}}</p>
        <p>紧急联系人</p>
        <p>张儿子：16601275207</p>
        <p>张女儿：16601275207</p>
        <p>张弟弟：16601275207</p>
        <p>关联设备</p>
        <p>主控设备：1111-0000-1111</p>
        <p>手环设备：1111-0000-1111</p>
        <p>心电设备：1111-0000-1111</p>
        <p>呼吸设备：1111-0000-1111</p>
        <p>下肢运动及机电：1111-0000-1111</p>
      </div>
      <!-- 呼吸情况 -->
      <div class="details-title">
          呼吸情况
      </div>
      <div class="details-main">
        <el-form label-width="120px">
          <el-form-item label="数据区间选择：">
            <el-select v-model="breatheDate" placeholder="请选择数据区间">
              <el-option label="9：00~9：40" value="shanghai"></el-option>
              <el-option label="11：00~12：00" value="beijing"></el-option>
            </el-select>
          </el-form-item>
          <el-form-item label="呼吸率：">
            <div>16～18次/分</div>
            <div class="tjx">心率图</div>
          </el-form-item>
        </el-form>
      </div>
    </div>
    <el-card>
      <h4>呼吸情况</h4>
      <el-form label-width="120px">
        <el-form-item label="数据区间选择：">
          <el-select v-model="breatheDate" placeholder="请选择数据区间">
            <el-option label="9：00~9：40" value="shanghai"></el-option>
            <el-option label="11：00~12：00" value="beijing"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="呼吸率：">
          <div>16～18次/分</div>
           <div class="tjx">心率图</div>
        </el-form-item>
      </el-form>
    </el-card>
    <el-card>
      <h4>心电情况</h4>
      <el-form>
        <el-form-item label="数据区间选择：">
          <el-select v-model="ecgDate" placeholder="请选择数据区间">
            <el-option label="9：00~9：40" value="shanghai"></el-option>
            <el-option label="11：00~12：00" value="beijing"></el-option>
          </el-select>
        </el-form-item>
        <div class="tjx">心电图</div>
        <el-form-item label="心率：">
          <div>60～100次/分</div>
        </el-form-item>
      </el-form>
    </el-card>
    <el-card>
      <h4>肢体情况</h4>
      <el-form>
        <div class="human">
          <el-form-item label="角度：">
            <div>30°</div>
          </el-form-item>
          <el-form-item label="幅度：">
            <div></div>
          </el-form-item>
        </div>
      </el-form>
      <div class="tjx">运动轨迹</div>
    </el-card>
    <el-card>
      <h4>肌电情况</h4>
      <el-form>
        <el-form-item label="数值：">
          <div>100</div>
        </el-form-item>
      </el-form>
    </el-card>
    <el-card>
      <h4>运动量</h4>
      <el-form>
        <div class="three">
          <el-form-item label="步数：">
            <div>500</div>
          </el-form-item>
          <el-form-item label="步频：">
            <div>20</div>
          </el-form-item>
          <el-form-item label="步幅：">
            <div>10</div>
          </el-form-item>
        </div>
        <div class="human">
          <el-form-item label="运动距离：">
            <div>1000m</div>
          </el-form-item>
          <el-form-item label="运动强度：">
            <div>中</div>
          </el-form-item>
        </div>
        <el-form-item label="消耗热量：">
          <div>252卡</div>
        </el-form-item>
      </el-form>
    </el-card>
    <el-card>
      <h4>体征数据</h4>
      <el-form>
        <div class="human">
          <el-form-item label="心率：">
            <div>60~100次/分</div>
          </el-form-item>
          <el-form-item label="呼吸率：">
            <div>60次/分</div>
          </el-form-item>
        </div>
      </el-form>
    </el-card>
    <el-card>
      <h4>异常数据</h4>
      <el-form>
        <el-form-item label="心电异常"></el-form-item>
      </el-form>
    </el-card>
    <el-card>
      <h4>运动状态</h4>
      <el-form>
        <el-form-item label="运动量少于范围"></el-form-item>
      </el-form>
    </el-card>
    <el-card>
      <h4>睡眠状态</h4>
      <el-form>
        <el-form-item label="睡眠良好"></el-form-item>
      </el-form>
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
      },
      breatheDate: "",
      ecgDate: "",
      dialogFormVisible: false
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
  background-color: #ccc;
}
.human {
  width: 36%;
  display: flex;
}
.human > div {
  flex: 1;
}
.three {
  width: 50%;
  display: flex;
}
.three > div {
  flex: 1;
}
</style>

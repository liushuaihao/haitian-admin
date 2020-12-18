<template>
  <el-card shadow="never" class="aui-card--fill">
    <el-card>
      <el-button type="success" v-if="$hasPermission('business:report:expor')">导出</el-button>
      <el-button type="success" v-if="$hasPermission('business:report:edit')">修改</el-button>
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
            <div>{{basicInformation.stature}}cm</div>
          </el-form-item>
          <el-form-item label="体重：">
            <div>{{basicInformation.weight}}kg</div>
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
      <h4>呼吸情况</h4>
      <el-form>
        <el-form-item label="数据区间选择：">
          <el-select v-model="breatheDate" placeholder="请选择数据区间">
            <el-option label="9：00~9：40" value="shanghai"></el-option>
            <el-option label="11：00~12：00" value="beijing"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="呼吸率：">
          <div>16～18次/分</div>
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
        <el-card class="tjx">心电图</el-card>
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
      <el-card class="tjx">
        运动轨迹
      </el-card>
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
          <el-form-item label="心电异常">
          </el-form-item>
      </el-form>
    </el-card>
    <el-card>
      <h4>运动状态</h4>
      <el-form>
          <el-form-item label="运动量少于范围">
          </el-form-item>
      </el-form>
    </el-card>
    <el-card>
      <h4>睡眠状态</h4>
      <el-form>
          <el-form-item label="睡眠良好">
          </el-form-item>
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
      ecgDate: ""
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
}
.human{
  width: 36%;
  display: flex;
}
.human>div{
  flex: 1;
}
.three{
  width: 50%;
  display: flex;
}
.three>div{
  flex: 1;
}
</style>

<template>
  <div>
    <chinaView v-if="isShow"/>
  </div>
</template>

<script>
import chinaView from './chinaView'
export default {
  components: {
    chinaView
  },
  data () {
    return {
      isShow: true,
    };
  },
  // 钩子函数  不了解的话 建议看看 vue的生命周期
  mounted () {
  },
  activated () {
    this.isShow = true;
  },
  deactivated () {
    this.isShow = false;
  }
};
</script>

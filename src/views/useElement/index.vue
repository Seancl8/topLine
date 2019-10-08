<template>
  <!-- 使用element-ui组件 -->
  <div class="useElement">
    <h3>这是使用element-ui的测试页面</h3>
    <h4>使用element-ui的按钮</h4>
    <el-row>
      <el-button>默认按钮</el-button>
      <el-button type="primary">主要按钮</el-button>
      <el-button type="success">成功按钮</el-button>
      <el-button type="info">信息按钮</el-button>
      <el-button type="warning">警告按钮</el-button>
      <el-button type="danger">危险按钮</el-button>
    </el-row>
    <h5>使用element-ui的开关</h5>
    <el-switch v-model="value" active-color="#13ce66" inactive-color="#ff4949"></el-switch>
  </div>
</template>

<script>
export default {
  data () {
    return {
      value: true
    }
  }
}
</script>

<style>
</style>

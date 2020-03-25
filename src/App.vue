<template>
  <div id="app">
    <div class="header">
      <el-select v-model="value" placeholder="请选择">
        <el-option
          v-for="item in options"
          :key="item.value"
          :label="item.label"
          :value="item.value"
        >
        </el-option>
      </el-select>
      <el-input v-model="input" placeholder="请输入URL"></el-input>
      <el-button type="primary">send</el-button>
    </div>
    <el-tabs v-model="activeName" @tab-click="handleClick">
      <el-tab-pane label="Params" name="get">Params</el-tab-pane>
      <el-tab-pane label="Body" name="post">Body</el-tab-pane>
    </el-tabs>
  </div>
</template>
<script>
export default {
  data() {
    return {
      options: [
        {
          value: "0",
          label: "get"
        },
        {
          value: "1",
          label: "post"
        }
      ],
      value: "",
      input: "",
      activeName: "get"
    };
  },
  methods: {
    handleClick(tab, event) {
      console.log(tab, event);
    }
  }
};
</script>
<style lang="less">
.header {
  display: flex;
}
</style>

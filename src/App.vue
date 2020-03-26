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
      <el-tab-pane label="Params" name="get">
        <p>{{ items }}</p>
        <table cellpadding="0" cellspacing="0" ref="table">
          <tr>
            <td>KEY</td>
            <td>VALUE</td>
          </tr>
          <tr v-for="(item, index) in items" :key="item.id">
            <td>
              <i class="el-icon-plus" @click="add"></i>
              <input v-model="item.key" />
            </td>
            <td>
              <input v-model="item.value" />
              <i class="el-icon-close" @click="remove(index)"></i>
            </td>
          </tr>
        </table>
      </el-tab-pane>
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
      activeName: "get",
      items: [{ id: 1, key: "", value: "" }]
    };
  },
  mounted() {
    this.axios({});
  },
  methods: {
    handleClick(tab, event) {
      console.log(tab, event);
    },
    add() {
      const length = this.items.length - 1;
      let _id = this.items[length].id + 1;
      this.items.push({ id: _id, key: "", value: "" });
    },
    remove(index) {
      this.items.splice(index, 1);
    }
  }
};
</script>
<style lang="less">
.header {
  display: flex;
}
table {
  border-collapse: collapse;
}
table tr td {
  position: relative;
  border: 1px solid #eaeaea;
  text-align: left;
  height: 30px;
  input {
    margin: 5px;
    border: 0;
  }
  i {
    display: none !important;
    padding: 0 10px;
    cursor: pointer;
  }
}
table tr:hover {
  i {
    display: inline-block !important;
  }
}
</style>

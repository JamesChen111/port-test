<template>
  <div id="app">
    <div class="header">
      <el-select v-model="value" placeholder="请选择请求类型">
        <el-option
          v-for="item in options"
          :key="item.value"
          :label="item.label"
          :value="item.value"
        >
        </el-option>
      </el-select>
      <el-input v-model="url" placeholder="请输入URL"></el-input>
      <el-button type="primary" @click="handleSubmit">send</el-button>
    </div>
    <el-tabs v-model="activeName">
      <el-tab-pane label="Params" name="get">
        <table cellpadding="0" cellspacing="0" style="width:100%">
          <tr>
            <td>KEY</td>
            <td>VALUE</td>
          </tr>

          <tr v-for="(item, index) in items" :key="item.id">
            <td>
              <i class="el-icon-plus" @click="add"></i>
              <input v-model="item.key" @input="changeValue" />
            </td>
            <td>
              <input v-model="item.value" @input="changeValue" />
              <i class="el-icon-close" @click="remove(index)"></i>
            </td>
          </tr>
        </table>
      </el-tab-pane>
      <el-tab-pane label="Body" name="post">
        <table cellpadding="0" cellspacing="0" style="width:100%">
          <tr>
            <td>KEY</td>
            <td>VALUE</td>
          </tr>
          <tr v-for="(item, index) in formData" :key="item.id">
            <td>
              <i class="el-icon-plus" @click="add1"></i>
              <input v-model="item.key" />
            </td>
            <td>
              <input v-model="item.value" />
              <i class="el-icon-close" @click="remove1(index)"></i>
            </td>
          </tr>
        </table>
      </el-tab-pane>
    </el-tabs>
  </div>
</template>
<script>
export default {
  data() {
    return {
      options: [
        {
          value: "get",
          label: "get"
        },
        {
          value: "post",
          label: "post"
        }
      ],
      value: "",
      url: "",
      activeName: "get",
      items: [],
      formData: [{ id: 1, key: "", value: "" }],
      headers: [{ id: 1, key: "", value: "" }],
      header: {}
    };
  },
  watch: {
    url(newValue) {
      const re_key = /(?:(?<=\?)|(?<=&))\w*\W*(?:(?==)|$)/g,
        re_value = /(?<==)\w*\W*(?:(?=&)|$)/g;
      const str_key = newValue.match(re_key),
        str_value = newValue.match(re_value);
      try {
        let i = 0;
        if (str_key == null && str_value == null) {
          this.items = [];
          this.items.push({ id: i++, key: "", value: "" });
        } else {
          const re = /^((https|http|ftp|rtsp|mms)?:\/\/)[^\s]+(?:\/|(?=\?))/;
          this.base_url = newValue.match(re)[0];
          const str_key_len = str_key.length - 1,
            str_value_len = str_value.length - 1;
          this.items = [];
          str_key.forEach((value, index) => {
            str_value.forEach((val, inx) => {
              if (index === inx) {
                this.items.push({ id: i++, key: value, value: val });
              }
            });
            if (index === str_key_len && str_key_len > str_value_len) {
              this.items.push({
                id: i++,
                key: value,
                value: ""
              });
            }
          });
        }
      } catch (error) {
        // console.log(error);
      }
    }
  },
  methods: {
    changeValue() {
      let URL,
        len = this.items.length - 1;
      if (this.base_url == undefined) {
        URL = this.url;
      } else {
        URL = this.base_url;
      }
      this.items.forEach((value, index) => {
        if (index == 0) {
          if (!value.value) {
            URL += "?" + value.key;
          } else URL += "?" + value.key + "=" + value.value;
        } else {
          if (!value.value && index === len) {
            URL += "&" + value.key;
          } else {
            URL += "&" + value.key + "=" + value.value;
          }
        }
      });
      this.url = URL;
    },
    add() {
      const length = this.items.length - 1;
      let _id = this.items[length].id + 1;
      this.items.push({ id: _id, key: "", value: "" });
    },
    add1() {
      const length = this.formData.length - 1;
      let _id = this.formData[length].id + 1;
      this.formData.push({ id: _id, key: "", value: "" });
    },
    remove(index) {
      const length = this.items.length;
      if (length > 1) {
        this.items.splice(index, 1);
      }
    },
    remove1(index) {
      const length = this.formData.length;
      if (length > 1) {
        this.formData.splice(index, 1);
      }
    },
    handleSubmit() {
      let URL = this.url;
      let requestBody = {};
      this.items.forEach((value, index) => {
        if (index === 0) {
          if (value.key !== "" && value.value !== "")
            URL += "?" + value.key + "=" + value.value;
        } else {
          URL += "&" + value.key + "=" + value.value;
        }
      });
      this.formData.forEach(value => {
        requestBody[value.key] = value.value;
      });
      this.$axios({
        method: this.value,
        url: URL,
        data: requestBody
      });
    }
  }
};
</script>
<style lang="less">
#app {
  width: 50%;
  height: 100vh;
  margin: 0 auto;
  border: 1px solid #dcdfe6;
  padding: 5px;
  box-sizing: border-box;
}
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
    width: 90%;
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

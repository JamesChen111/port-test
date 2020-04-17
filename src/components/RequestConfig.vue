<template>
  <table cellpadding="0" cellspacing="0" style="width:100%">
    <tr>
      <td>KEY</td>
      <td>VALUE</td>
    </tr>
    <tr v-for="(item, index) in config" :key="item.id">
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
</template>

<script>
export default {
  props: {
    config: {
      type: Array,
      default: () => {}
    }
  },
  data() {
    return {};
  },
  methods: {
    add() {
      const length = this.config.length - 1;
      let _id = this.config[length].id + 1;
      this.config.push({ id: _id, key: "", value: "" });
    },
    remove(index) {
      const length = this.config.length;
      if (length > 1) {
        this.config.splice(index, 1);
      }
    }
  }
};
</script>

<style scoped></style>

<template>
  <el-form-item class="form-item">
    <el-input
      v-model="item.value"
      :placeholder="item.label"
      clearable
      @clear="clear"
      @change="(p) => $emit('change', p)"
    />
    <el-divider content-position="right" class="labelText">{{
      item.label
    }}</el-divider>
  </el-form-item>
</template>
<script>
export default {
  name: 'FormItem',
  props: {
    item: {
      type: Object,
      default: () => {},
    },
  },
  methods: {
    clear() {
      this.item.value = null
    },
  },
}
</script>
<style lang="scss">
.el-input__inner {
  background-color: var(--color-black);
  color: var(--color-white);
  border-color: var(--color-black);
  &:focus,
  &:hover {
    border-color: var(--color-black) !important;
  }
  &::placeholder {
    color: var(--color-text-secondary);
  }
}
.el-select {
  &:hover {
    color: var(--color-black);
  }
  .el-input {
    width: 130px;
    &.is-focus {
      .el-input__inner {
        border-color: var(--color-black);
      }
    }
  }
}
.el-form-item {
  &.form-item {
    margin-right: 2px;
  }
}
</style>

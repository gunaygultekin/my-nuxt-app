<template>
  <el-form-item class="form-item">
    <el-input
      v-model="form.value"
      class="input-with-select"
      :placeholder="form.label"
      clearable
      @clear="clear"
    />
    <el-divider content-position="right" class="labelText">{{
      form.label
    }}</el-divider>
  </el-form-item>
</template>
<script>
export default {
  name: 'FormItem',
  props: {
    form: {
      type: Object,
      default: () => {},
    },
  },
  methods: {
    clear() {
      this.form.value = null
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

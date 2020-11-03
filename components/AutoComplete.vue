<template>
  <el-form-item class="form-item">
    <el-autocomplete
      v-model="predictions"
      class="input-with-select"
      placeholder="Address"
      :fetch-suggestions="querySearchAsync"
      :trigger-on-focus="false"
      :minlength="3"
      clearable
      @clear="clear"
      @select="handleSelect"
    />
    <el-divider content-position="right" class="labelText">Address</el-divider>
  </el-form-item>
</template>
<script>
export default {
  name: 'FormItem',
  data() {
    return {
      predictions: '',
    }
  },
  methods: {
    clear() {
      this.predictions = ''
    },
    querySearchAsync(queryString, cb) {
      this.$axios
        .$get(
          `/gapi/maps/api/place/autocomplete/json?input=${queryString}&key=${process.env.googleMapAPIKey}`
        )
        .then((res) => {
          const { predictions } = res
          cb(
            predictions.map((p) => ({
              value: p.description,
              data: p,
            }))
          )
        })
    },
    handleSelect(item) {
      this.$emit('handleSelect', item)
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

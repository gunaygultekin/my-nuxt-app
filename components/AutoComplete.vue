<template>
  <el-form-item class="form-item">
    <el-autocomplete
      ref="autocomplete"
      v-model="predictions"
      class="input-with-select"
      placeholder="Address"
      :fetch-suggestions="querySearchAsync"
      :trigger-on-focus="triggerOnFocus"
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
  props: {
    searchText: {
      type: String,
      default: '',
    },
  },
  data() {
    return {
      predictions: '',
      triggerOnFocus: false,
    }
  },
  watch: {
    searchText: {
      immediate: true,
      handler(value) {
        if (value) {
          this.predictions = value
          this.triggerOnFocus = true
          this.$refs.autocomplete.focus()
        }
      },
    },
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
          this.triggerOnFocus = false
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
<style lang="scss" scoped>
.input-with-select {
  width: 450px;
}
</style>

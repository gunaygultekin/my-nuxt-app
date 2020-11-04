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
      @select="select"
      @mouseover="mouseover"
    />
    <el-divider content-position="right" class="labelText">Address</el-divider>
  </el-form-item>
</template>
<script>
import { Message } from 'element-ui'

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
      isCleared: false,
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
        if (this.isCleared) {
          console.log('do this')
          this.isCleared = false
        }
      },
    },
  },
  methods: {
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
        .catch((error) => {
          Message({
            message: error,
            type: 'warning',
            duration: 5 * 1000,
          })
        })
    },
    select(p) {
      this.$emit('select', p)
    },
    clear() {
      this.isCleared = true
      this.$emit('clear')
      this.$refs.autocomplete.$el.blur()
    },
    mouseover(p) {
      this.$emit('mouseover', this.predictions)
    },
  },
}
</script>
<style lang="scss" scoped>
.input-with-select {
  width: 450px;
}
</style>

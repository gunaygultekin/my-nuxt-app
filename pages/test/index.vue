<template>
  <div style="width: 500px; margin: 0 auto">
    <el-row>
      <el-col :span="14" :offset="6">
        <span>TEST - FIELD TO INPUT AN ADDRESS</span>
      </el-col>
    </el-row>
    <el-form :inline="true" :model="form">
      <el-form-item>
        <el-select v-model="form.addressType" placeholder="Select">
          <el-option
            v-for="item in options"
            :key="item.value"
            :label="item.label"
            :value="item.value"
          >
          </el-option>
        </el-select>
      </el-form-item>
      <el-form-item>
        <el-input
          v-model="form.searchText"
          class="input-with-select"
          placeholder="Address"
          clearable
        />
        <el-divider content-position="right" class="labelText"
          >address</el-divider
        >
      </el-form-item>
    </el-form>
  </div>
</template>

<script>
export default {
  name: 'Test',
  data() {
    return {
      form: {
        addressType: null,
        searchText: '',
      },
      options: [
        {
          value: 'residential',
          label: 'Residential',
        },
        {
          value: 'domicile',
          label: 'Domicile',
        },
        {
          value: 'legal',
          label: 'Legal',
        },
        {
          value: 'operational',
          label: 'Operational',
        },
      ],
    }
  },
  mounted() {
    this.addressType = this.options[0].value // set the first item as default
  },
}
</script>
<style lang="scss">
body {
  background: var(--color-black);
}
.el-row {
  margin-bottom: 20px;
  &:last-child {
    margin-bottom: 0;
  }
}
.el-col {
  border-radius: 4px;
}
.el-input__inner {
  background-color: var(--color-black);
  color: var(--color-white);
  border-color: var(--color-black);
  &:focus,
  &:hover {
    border-color: var(--color-black);
  }
  &::placeholder {
    color: var(--color-text-secondary);
  }
}
.el-select .el-input {
  width: 130px;
}
.input-with-select .el-input-group__prepend {
  background-color: var(--color-black);
  border-color: var(--color-black);
}
.el-select-dropdown {
  border: 0px solid var(--color-black);

  background-color: var(--color-black);
}
.el-select-dropdown__list {
  background: var(--color-text-regular);
  .el-select-dropdown__item {
    background-color: var(--color-text-regular);
    color: var(---color-text-secondary);
    :hover,
    &.hover {
      background-color: var(--border-color-base);
      color: var(--color-black);
    }

    &.selected {
      color: var(--color-white);
    }
  }
}
.el-scrollbar {
  border-bottom-right-radius: 10px;
  border-bottom-left-radius: 10px;
}

.labelText {
  text-transform: uppercase;
  margin-top: -5px;

  .el-divider__text {
    background-color: var(--color-black);
    color: var(--color-white);
    padding: 0 3px;
    line-height: 17px;
  }
}
</style>

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
        <el-divider content-position="right" class="labelText"></el-divider>
      </el-form-item>
      <el-form-item style="margin-top: 15px">
        <el-tooltip
          class="question-tooltip"
          effect="dark"
          :content="questionInfo"
          placement="bottom-start"
          :disabled="questionInfo === null"
        >
          <el-button>
            <font-awesome-icon
              :disabled="questionInfo === null"
              :icon="
                isHover && questionInfo !== null
                  ? 'question-circle'
                  : 'question'
              "
              :class="
                isHover && questionInfo !== null
                  ? 'question-icon-activated'
                  : 'question-icon'
              "
              @mouseover="handleMouseOver"
              @mouseleave="isHover = false"
            />
          </el-button>
        </el-tooltip>
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
      isHover: false,
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
      questionInfo: null,
      // questionInfo: 'Usefull Tooltip Content',
    }
  },
  mounted() {
    this.addressType = this.options[0].value // set the first item as default
  },
  methods: {
    handleMouseOver() {
      if (this.questionInfo === null) this.isHover = false
      else this.isHover = true
    },
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
  .el-col {
    border-radius: 4px;
  }
}
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
.el-select-dropdown {
  border: 0px solid var(--color-black);
  background-color: var(--color-black);
  &__list {
    background: var(--color-dropdown-gray);
    .el-select-dropdown__item {
      background-color: var(--color-dropdown-gray);
      color: var(--color-white);
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
.question-icon {
  color: var(--color-text-primary);
  &-activated {
    color: var(--color-text-regular);
  }
}
.question-tooltip {
  color: transparent !important;
  background-color: transparent !important;
  border: 0;
  padding: 0;
}
:focus {
  outline: var(--color-black);
}
.el-popper[x-placement^='bottom'] {
  .popper__arrow {
    display: none;
  }
}
</style>

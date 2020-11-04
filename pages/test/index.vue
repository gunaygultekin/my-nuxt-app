<template>
  <div style="width: 95%; margin: 0 auto">
    <el-row>
      <el-col :span="14" :offset="6">
        <span>TEST - FIELD TO INPUT AN ADDRESS</span>
      </el-col>
    </el-row>
    <el-form :inline="true" :model="form">
      <el-form-item>
        <el-select v-model="form.type" placeholder="Select">
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
      <el-form-item class="question">
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
      <AutoComplete
        :searchText="searchText"
        @handleSelect="handleSelect"
      ></AutoComplete>
      <FormItem
        v-if="form.item !== null"
        :item="form.item"
        @change="handleChange"
      ></FormItem>
      <el-form-item v-if="isPlusButtonShowed" class="plus-buton">
        <font-awesome-icon icon="plus" class="font-icon" @click="click" />
      </el-form-item>
    </el-form>
  </div>
</template>

<script>
import FormItem from '@/components/FormItem'
import AutoComplete from '@/components/AutoComplete'

// street, house number, zip code, town, major town, country and also the GPS (geographical) coordinates
const itemMap = new Map([
  [0, 'N⁰'],
  [1, 'Zip Code'],
  [2, 'Town'],
  [3, 'Major Town'],
  [4, 'Country'],
  [5, 'GPS Coordinate'],
])

export default {
  name: 'Test',
  components: {
    FormItem,
    AutoComplete,
  },
  data() {
    return {
      form: {
        type: null,
        item: null,
      },
      searchText: null,
      isHover: false,
      isPlusButtonShowed: false,
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
      additionalEntryIndex: 0,
    }
  },
  computed: {
    isShow() {
      const { items } = this.form
      let isAllFilled = false
      if (items.length <= itemMap.size) {
        items.forEach((item) => {
          if (Boolean(item.value) === true) {
            // it is not empty
            isAllFilled = true
          } else isAllFilled = false
        })
      }
      return isAllFilled
    },
  },
  mounted() {
    this.form.type = this.options[0].value // set the first item as default
  },
  methods: {
    handleMouseOver() {
      if (this.questionInfo === null) this.isHover = false
      else this.isHover = true
    },
    click() {
      this.isPlusButtonShowed = false
      const itemText = itemMap.get(this.additionalEntryIndex)
      this.form.item = {
        label: itemText,
        value: null,
      }
      this.additionalEntryIndex += 1
    },
    handleSelect(p) {
      this.isPlusButtonShowed = true
      const { value } = p
      this.searchText = value
    },
    handleChange(p) {
      this.isPlusButtonShowed = false
      this.searchText = p + ',' + this.searchText
      this.form.item = null
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
.el-form-item {
  &.question {
    margin-top: 15px;
  }
  &.plus-buton {
    margin-top: 15px;
    &:hover {
      cursor: pointer;
    }
    .font-icon {
      color: var(--color-white);
    }
  }
}
</style>

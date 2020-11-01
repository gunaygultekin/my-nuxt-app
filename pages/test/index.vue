<template>
  <div style="width: 90%; margin: 0 auto">
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
      <FormItem
        v-for="(item, index) in form.items"
        :key="index"
        :item="item"
      ></FormItem>
      <el-form-item v-if="isShow" class="plus-buton">
        <font-awesome-icon icon="plus" class="font-icon" @click="click" />
      </el-form-item>
    </el-form>
  </div>
</template>

<script>
import FormItem from '@/components/FormItem'

// street, house number, zip code, town, major town, country and also the GPS (geographical) coordinates
const itemMap = new Map([
  [1, 'Number'],
  [2, 'Zip Code'],
  [3, 'Town'],
  [4, 'Major Town'],
  [5, 'Country'],
  [6, 'GPS Coordinate'],
])

export default {
  name: 'Test',
  components: {
    FormItem,
  },
  data() {
    return {
      form: {
        type: null,
        items: [],
      },
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
  created() {
    // add default address field
    this.form.items.push({
      label: 'Address',
      value: null,
    })
  },
  mounted() {
    this.type = this.options[0].value // set the first item as default
  },
  methods: {
    handleMouseOver() {
      if (this.questionInfo === null) this.isHover = false
      else this.isHover = true
    },
    click() {
      const mapIndex = this.form.items.length
      const itemText = itemMap.get(mapIndex)
      this.form.items.push({
        label: itemText,
        value: null,
      })

      /* const { items } = this.form
      const mapIndex = items.length
      const itemText = itemMap.get(mapIndex)
      items.push({
        label: itemText,
        value: null,
      })

      this.isPlusButtonShowed = false // clear
      if (items.length <= itemMap.size) {
        items.forEach((item) => {
          if (Boolean(item.value) === true) {
            // it is not empty
            this.isPlusButtonShowed = true
          }
        })
      } */
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

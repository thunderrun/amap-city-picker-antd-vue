<template>
  <div class="citypicker">
    <div class="select-group">
      <div
        class="title"
        v-text="title"
      />
      <div class="action">
        <i
          class="el-icon-delete"
          @click="$emit('confirm',[])"
        />
      </div>
    </div>
    <div class="tab-group">
      <div
        v-for="(tab,i) in tabs"
        :key="i"
        :class="{'active':i==currentTab}"
        class="tab"
        @click="choseTab(i)"
      >
        <div
          class="ellipsis"
          v-text="tab"
        />
      </div>
    </div>
    <div class="items">
      <div
        v-if="parent"
        class="item"
        @click="choseAll"
        v-text="'全部'"
      />
      <div
        v-for="(item,i) in items"
        :key="i"
        class="item"
        :class="isSelected(item)"
        @click="choseItem(item)"
      >
        <div
          class="ellipsis"
          v-text="item.label"
        />
      </div>
    </div>
  </div>
</template>

<script>
const [PROVINCE, CITY, AREA] = [0, 1, 2];

export default {
  props: {
    value: {
      type: Array,
      default: () => [],
    },
    title: {
      type: String,
      default: '选择地址',
    },
    deep: {
      type: [Number, String],
      default: 3,
    },
    visible: {
      type: Boolean,
      default: false,
    },
    options: {
      type: Array,
      default: () => [],
    },
  },
  data() {
    return {
      current: [],
      currentTab: PROVINCE,
      list: {
        [PROVINCE]: [],
        [CITY]: [],
        [AREA]: [],
      },
    };
  },
  computed: {
    items() {
      return this.currentTab === PROVINCE
        ? this.options
        : this.list[this.currentTab];
    },
    tabs() {
      const { current } = this;
      return ['省', '市', '区']
        .map((v, i) => (current[i] ? current[i].label : v))
        .slice(0, this.deep);
    },
    parent() {
      const { currentTab } = this;
      if (currentTab > PROVINCE) {
        return this.current[currentTab - 1];
      }
      return '';
    },
  },
  watch: {
    visible(v) {
      if (v) {
        this.current = this.value;
        this.currentTab = Math.max(0, this.value.length - 1);
      }
    },
  },
  methods: {
    isSelected(item) {
      return this.current.includes(item) ? 'active' : '';
    },
    choseTab(tabType) {
      if (this.list[tabType].length === 0 && tabType !== PROVINCE) {
        return;
      }
      switch (tabType) {
        case PROVINCE:
          this.currentTab = PROVINCE;
          this.current = [this.current[0]];
          break;
        case CITY:
          if (this.currentTab === AREA) {
            this.currentTab = CITY;
            this.current = this.current.slice(0, 2);
          }
          break;
        default:
      }
    },
    choseItem(item, index = this.currentTab) {
      const nextTab = index + 1;
      const { children } = item;
      if (index) {
        this.current.splice(index, 1, item);
      } else {
        this.current = [item];
      }
      if (children && children.length > 0) {
        if (children.length > 1) {
          this.list[nextTab] = children;
          const { deep } = this;
          if (nextTab < deep) {
            this.currentTab = nextTab;
          } else {
            this.$emit('confirm', this.current);
          }
        } else {
          this.list[nextTab] = [];
          this.choseItem(children[0], nextTab);
        }
      } else {
        this.$emit('confirm', this.current);
      }
    },
    choseAll() {
      this.$emit(
        'confirm',
        this.current.slice(0, Math.max(0, this.currentTab)),
      );
    },
  },
};
</script>

<style lang='less' scoped>
  .citypicker {
    width: 420px;
    display: flex;
    flex-direction: column;
    .item {
      text-align: center;
      color: #333;
    }
  }
  .select-group {
    background: #fff;
    display: flex;
    justify-content: space-between;
    align-items: center;
    .title {
      color: #333;
      font-weight: bold;
      text-align: left;
      font-size: 14px;
      line-height: 24px;
      padding: 0 10px;
      box-sizing: border-box;
    }
    .action {
      justify-content: flex-end;
      i {
        cursor: pointer;
        &:hover {
          color: #409eff;
        }
      }
    }
  }

  .tab-group {
    height: 40px;
    padding: 8px 0 0;
    box-sizing: border-box;
    display: flex;
    border-bottom: solid 1px #EBEEF5;
    .tab {
      cursor: pointer;
      display: flex;
      align-items: center;
      justify-content: center;
      border-radius: 6px 6px 0 0;
      width: 80px;
      height: 100%;
      margin-right: 5px;
      font-size: 14px;
      background: #EEF0F3;
      color: #333;
      &.active {
        color: #fff;
        background: #409eff;
      }
    }
  }
  .items {
    display: flex;
    flex-wrap: wrap;
    .item {
      width: 60px;
      cursor: pointer;
      box-sizing: border-box;
      height: 40px;
      line-height: 40px;
      font-size: 14px;
      &:hover, &.active {
        color: #409eff;
        font-weight: bold;
      }
    }
  }
  .ellipsis {
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
  }
</style>

<template>
  <a-popover
    v-model="showPicker"
    placement="bottomLeft"
  >
    <Panel
      slot="content"
      :value="selected"
      :visible="showPicker"
      :options="fieldOption"
      :deep="level"
      @confirm="onConfirm"
    />
    <a-input
      class="citypicker-input"
      read-only
      :value="view"
      :placeholder="placeholder"
    />
  </a-popover>
</template>

<script>
import Panel from './Panel.vue';
import cityData from './data';

export default {
  name: 'CityPicker',
  components: { Panel },
  props: {
    value: {
      type: [String, Number, Object, Array],
      default: '',
    },
    level: {
      type: [Number, String],
      default: 3,
    },
    placeholder: {
      type: String,
      default: '请选择地点',
    },
  },
  data() {
    return {
      showPicker: false,
      fieldOption: cityData,
    };
  },
  computed: {
    selected() {
      const { value } = this;
      const option = this.fieldOption;
      if (value instanceof Array) {
        const copy = value.slice();
        let children = option;
        let val = copy.shift();
        const items = [];
        while (val && children) {
          // eslint-disable-next-line no-loop-func
          const target = children.find((item) => item.value === val);
          children = target ? target.children : undefined;
          val = copy.shift();
          if (target) {
            items.push(target);
          }
        }
        return items;
      } if (value) {
        const item = option.find((item2) => item2.value === value);
        return item ? [item] : [];
      }
      return [];
    },
    view() {
      if (this.value === 'All') {
        return '全部';
      }
      return this.selected && this.selected.length
        ? this.selected.map((item) => item.label).join('/')
        : '';
    },
  },
  methods: {
    onConfirm($event) {
      this.showPicker = false;
      const val = $event && $event.length
        ? $event.map((item) => item.value)
        : [this.fieldOption[0].value];
      this.$emit('input', val);
    },
  },
};
</script>

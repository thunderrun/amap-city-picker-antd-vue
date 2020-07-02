# Antd Design Vue 高德地图城市(编码)选择组件

## Example

```javascript
<template>
  <div id="app">
    <RegionPicker
      v-model="cityCode"
      :level="3"
      placeholder="请选择地点"
    />
  </div>
</template>

<script>
import RegionPicker from 'amap-city-picker-antd-vue';

export default {
  name: 'App',
  components: {
    RegionPicker,
  },
  data() {
    return {
      cityCode: [320000, 320100, 320114],
    };
  },
};
</script>
```


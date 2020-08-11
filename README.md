# Antd Design Vue 高德地图城市(编码)选择组件

## Prerequisites

- Antd Design Vue

## Usage

```cmd
npm install amap-region-picker-antd-vue
```

```javascript
<template>
  <div id="app">
    <RegionPicker
      v-model="cityCode"
      :level="3" // 精确到省:1, 市:2, 区:3
      placeholder="请选择地点"
    />
  </div>
</template>

<script>
import RegionPicker from 'amap-region-picker-antd-vue';

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


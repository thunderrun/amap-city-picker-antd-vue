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
import RegionPicker from '@/components/RegionPicker/RegionPicker.vue';

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

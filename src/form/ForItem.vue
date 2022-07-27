<template>
  <el-form-item v-if="item" :label="item.label" :prop="item.prop">
    <template v-if="item.type === 'text'">
      <el-input
        v-bind="$attrs"
        v-on="$listeners"
        :placeholder="item.placeholder || '请输入'"
      ></el-input>
    </template>
    <template v-if="item.type === 'select'">
      <el-select
        v-bind="$attrs"
        v-on="$listeners"
        :placeholder="item.placeholder"
      >
        <el-option
          v-for="(o, i) in item.options"
          :key="i"
          :label="o.label"
          :value="o.value"
        ></el-option>
      </el-select>
    </template>
    <template v-if="['date', 'daterange'].includes(item.type)">
      <el-date-picker
        v-bind="$attrs"
        v-on="$listeners"
        :type="item.type"
        :value-format="item.format || 'yyyyMMdd'"
        :placeholder="item.placeholder || '请选择日期'"
        start-placeholder="开始日期"
        end-placeholder="结束日期"
      >
      </el-date-picker>
    </template>
  </el-form-item>
</template>

<script>
export default {
  name: 'ForItem',
  props: {
    item: {
      type: Object,
      default: () => {}
    }
  },
  data() {
    return {}
  },
  methods: {}
}
</script>

<style lang="scss" scoped>
.el-form-item {
  margin-bottom: 0;
  margin-right: 20px;
}
</style>

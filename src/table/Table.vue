<template>
  <div>
    <el-table :data="tableData" @selection-change="tableSelection">
      <template v-for="(item, i) in tableColData">
        <el-table-column
          v-if="item.type === 'selection'"
          :key="i"
          :align="item.align || 'left'"
          type="selection"
          :width="item.width"
        ></el-table-column>
        <el-table-column
          v-else-if="item.type === 'config'"
          :key="i"
          :align="item.align || 'left'"
          :label="item.label"
          :width="item.width"
        >
          <template #default="{ row }">
            <el-button
              v-for="(btn, index) in item.buttons"
              :key="index"
              :type="btn.type"
              :size="btn.size"
              @click="configBtnClick(btn.action, row)"
              >{{ btn.label }}</el-button
            >
          </template>
        </el-table-column>
        <el-table-column
          v-else
          :key="i"
          :align="item.align || 'left'"
          :label="item.label"
          :prop="item.prop"
          :width="item.width"
          :formatter="item.formatter"
        >
        </el-table-column>
      </template>
    </el-table>
  </div>
</template>

<script>
export default {
  name: 'Table',
  props: {
    tableColData: {
      type: Array,
      default: () => []
    },
    tableData: {
      type: Array,
      default: () => []
    }
  },
  data() {
    return {}
  },
  methods: {
    tableSelection(selection) {
      this.$emit('tableSelection', selection)
    },
    configBtnClick(action, row) {
      this.$emit('configBtnClick', { action, row })
    }
  }
}
</script>

<style lang="scss" scoped></style>

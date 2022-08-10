<template>
  <div>
    <el-table
      :data="tableData"
      @selection-change="tableSelection"
      border
      style="width: 100%"
      :height="tableHeight || '300px'"
    >
      <template v-for="(item, i) in tableColData">
        <el-table-column
          v-if="['selection', 'index'].includes(item.type)"
          :key="i"
          :align="item.align || 'left'"
          :label="item.label"
          :type="item.type"
          :width="item.width || '80px'"
        ></el-table-column>
        <el-table-column
          v-else-if="item.type === 'config'"
          :key="i"
          :align="item.align || 'left'"
          :label="item.label"
          :prop="item.prop"
          :width="item.width || '80px'"
          :fixed="item.fixed"
        >
          <template #default="{ row }">
            <template v-if="row.permissionList">
              <el-button
                class="btn"
                v-for="(btn, index) in row.permissionList"
                :key="index"
                :type="btn.type"
                :size="btn.size"
                @click="configBtnClick(btn.action, row)"
                >{{ btn.label }}</el-button
              >
            </template>
            <template v-else>
              <el-button
                class="btn"
                v-for="(btn, index) in item.buttons"
                :key="index"
                :type="btn.type"
                :size="btn.size"
                @click="configBtnClick(btn.action, row)"
                >{{ btn.label }}</el-button
              >
            </template>
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
          :fixed="item.fixed"
        >
        </el-table-column>
      </template>
    </el-table>
    <!-- <div v-else>暂无数据！！！</div> -->
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
    },
    tableHeight: {
      type: String,
      default: () => '300px'
    }
  },
  create() {
    console.log(this.tableHeight)
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

<style lang="scss" scoped>
.btn {
  margin: 0 6px 0 0;
}
</style>

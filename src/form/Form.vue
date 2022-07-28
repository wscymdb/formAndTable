<template>
  <div>
    <el-form
      :inline="formInfo.inline"
      :label-width="formInfo.labelWidth || '60px'"
      label-suffix=":"
      v-if="formInfo.data"
      ref="formRef"
      :rules="formInfo.rules"
      :model="dataForm"
    >
      <template v-for="(item, i) in formInfo.data">
        <ForItem
          :key="i"
          :item="item"
          :class="{ blockable: !formInfo.inline }"
          v-model="dataForm[item.prop]"
          @fileList="setFileList"
        />
      </template>
    </el-form>
  </div>
</template>

<script>
import ForItem from './ForItem.vue'
export default {
  name: 'Form',
  props: {
    formInfo: {
      type: Object,
      default: () => {}
    }
  },
  components: {
    ForItem
  },
  data() {
    return {
      dataForm: {},
      fileList: []
    }
  },
  methods: {
    // 重置
    resetFields() {
      this.$refs.formRef.resetFields()
    },
    // setFileList
    setFileList(fileList) {
      this.fileList = fileList
    },
    getFileList() {
      return this.fileList
    }
  },
  watch: {
    dataForm: {
      handler(curr) {
        this.$emit('update:value', curr)
      },
      deep: true
    }
  }
}
</script>

<style lang="scss" scoped>
.blockable {
  margin: 5px 0;
  ::v-deep .w100 {
    width: 100%;
  }
}
</style>

<template>
  <div>
    <el-form
      :label-width="formInfo.labelWidth"
      :label-suffix="formInfo.labelSuffix"
      v-if="formInfo.data"
      ref="formRef"
      :class="{ flexable: formInfo.inline }"
      :rules="formInfo.rules"
      :model="dataForm"
    >
      <template v-for="(item, i) in formInfo.data">
        <ForItem
          :key="i"
          :item="item"
          :class="{ blockable: formInfo.inline }"
          v-model="dataForm[item.prop]"
          ref="formItemRef"
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
    },
    dataInfo: {
      type: Object
      // default: () => {}
    }
  },
  components: {
    ForItem
  },
  provide() {
    return {
      formProvide: this
    }
  },
  created() {
    // if (this.dataInfo) {
    //   this.dataForm = this.dataInfo
    // }
  },
  data() {
    return {
      dataForm: {
        fileList: [],
        files: [],
        deleteFilesList: []
      }
    }
  },
  methods: {
    // 重置
    resetFields() {
      this.$refs.formRef.resetFields()
    },

    // 外界获取以上传的文件列表
    getFileList() {
      return this.files
    },
    // 回显文件列表
    setItemList(fileList) {
      this.fileList = fileList
    },
    //
    downloadFile(down) {
      this.$emit('downloadFile', down)
    }
  },
  watch: {
    dataForm: {
      handler(curr) {
        this.$emit('update:value', curr)
      },
      deep: true
    },
    dataInfo: {
      handler(curr) {
        if (curr) {
          this.dataForm = this.dataInfo
        }
      },
      deep: true,
      immediate: true
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
.flexable {
  display: flex;
  flex-wrap: wrap;
}
</style>

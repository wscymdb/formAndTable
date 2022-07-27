<template>
  <div>
    <el-form
      inline
      label-suffix=":"
      v-if="formInfo.data"
      ref="formRef"
      :rules="formInfo.rules"
      :model="dataForm"
    >
      <template v-for="(item, i) in formInfo.data">
        <ForItem :key="i" :item="item" v-model="dataForm[item.prop]" />
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
      dataForm: {}
    }
  },
  methods: {
    // 重置
    resetFields() {
      this.$refs.formRef.resetFields()
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

<style lang="scss" scoped></style>

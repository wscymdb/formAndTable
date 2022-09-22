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
import { defaultFormat } from "moment";
import ForItem from "./ForItem.vue";
export default {
  name: "Form",
  props: {
    formInfo: {
      type: Object,
      default: () => {},
    },
    dataInfo: {
      type: Object,
      // default: () => {}
    },
  },
  components: {
    ForItem,
  },
  provide() {
    return {
      formProvide: this,
    };
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
        deleteFilesList: [],
      },
    };
  },
  methods: {
    // 重置
    resetFields() {
      let keys = Object.keys(this.dataForm);
      keys.forEach((item, i, arr) => {
        this.$set(this.dataForm, item, "");
      });
    },
    validate(fn) {
      this.$refs.formRef.validate((valid) => {
        fn(valid);
      });
    },

    // 外界获取以上传的文件列表
    getFileList() {
      return this.files;
    },
    // 回显文件列表
    setItemList(fileList) {
      this.fileList = fileList;
    },
    //
    downloadFile(down) {
      this.$emit("downloadFile", down);
    },
    // 自定义label，icon的点击事件
    labelIconClick(item) {
      this.$emit("labelIconClick", item);
    },
    // cascader  change
    cascaderChange(item) {
      this.$emit("cascaderChange", item);
    },
  },
  watch: {
    dataForm: {
      handler(curr) {
        this.$emit("update:value", curr);
      },
      deep: true,
    },
    dataInfo: {
      handler(curr) {
        if (curr) {
          this.dataForm = this.dataInfo;
        }
      },
      deep: true,
      immediate: true,
    },
  },
};
</script>

<style lang="scss" scoped>
.blockable {
  margin: 8px 0;
  ::v-deep .w100 {
    width: 100%;
  }
}
.flexable {
  display: flex;
  flex-wrap: wrap;
}
</style>

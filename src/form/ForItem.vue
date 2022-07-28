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
        class="w100"
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
    <template v-if="['date', 'daterange', 'datetime'].includes(item.type)">
      <el-date-picker
        class="w100"
        v-bind="$attrs"
        v-on="$listeners"
        :type="item.type"
        :value-format="item.format"
        :placeholder="item.placeholder || '请选择日期'"
        start-placeholder="开始日期"
        end-placeholder="结束日期"
      >
      </el-date-picker>
    </template>
    <template v-if="item.type === 'textarea'">
      <el-input
        type="textarea"
        v-bind="$attrs"
        v-on="$listeners"
        :rows="item.row || 3"
        :max="10"
        resize="none"
        :placeholder="item.placeholder || '请输入内容'"
      >
      </el-input>
    </template>
    <template v-if="item.type === 'upload'">
      <el-upload
        class="upload-demo"
        action=""
        multiple
        v-bind="$attrs"
        v-on="$listeners"
        :on-remove="handleRemove"
        :before-upload="(file) => beforeUpload(file, item.fileTypeList)"
        :http-request="(file) => uploadFile"
        :limit="item.limit"
        :file-list="fileList"
      >
        <el-button size="small" type="primary" icon="el-icon-upload2"
          >点击上传</el-button
        >
        <div slot="tip" class="el-upload__tip" v-if="item.fileTypeList">
          只能上传{{
            item.fileTypeList.join('/')
          }}类型文件，且不超过5MB,最多上传{{ item.limit }}个
        </div>
      </el-upload>
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
    return {
      fileList: [],
      files: [],
      fileSize: 5 * 1024 * 1024 // 5MB
    }
  },
  methods: {
    // 文件列表移除文件时的钩子
    handleRemove(file, fileList) {
      console.log('===>', file)
      let { status, name } = file
      if (status === 'success') {
        this.fileList = this.fileList.filter((item) => item.name !== name)
        this.files = this.files.filter((item) => item.name !== name)
      }
    },

    beforeUpload(file, fileTypeList) {
      console.log('===>beforeUpload', file)
      let name = file.name
      // 判断文件类型
      if (fileTypeList.length && !fileTypeList.includes(file.type)) {
        this.$message.info('请上传jpeg、jpg、png类型的图片')
        return false
      }
      // 判断文件大小
      if (file.size > this.fileSize) {
        this.$message.info('文件大小需在5M以内')
        return false
      }
      // 判断是否重名
      let isOnly = this.fileList.some((item) => name === item.name)

      if (isOnly) {
        this.$message.info(`请勿上传同名文件${name}`)
        return false
      }
    },
    // 文件上传前的钩子
    uploadFile({ file }) {
      // debugger
      console.log(file)

      const url = this.getObjectURL(file)

      this.fileList.push({ name, url })
      this.files.push(file)
    },
    // 获取文件流的地址
    getObjectURL(data) {
      var url = null
      if (window.createObjectURL !== undefined) {
        // basic
        url = window.createObjectURL(data)
      } else if (window.webkitURL !== undefined) {
        // webkit or chrome
        try {
          url = window.webkitURL.createObjectURL(data)
        } catch (error) {
          console.log(error)
        }
      } else if (window.URL !== undefined) {
        // mozilla(firefox)
        try {
          url = window.URL.createObjectURL(data)
        } catch (error) {
          console.log(error)
        }
      }
      return url
    }
  },
  watch: {
    fileList(curr) {
      this.$emit('fileList', this.files)
    }
  }
}
</script>

<style lang="scss" scoped>
.el-form-item {
  margin-bottom: 0;
  margin-right: 20px;
}
</style>

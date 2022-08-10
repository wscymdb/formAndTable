# form表单

**基于element ui的二次封装，里面的类型什么的都```和element一样```，这里就不过多赘述**

## 例子

```html
<QueryForm :formInfo="formInfo" :value.sync="dataForm" ref="queryFormRef" />
```



##  Form Attributes（属性）

| 名称        | 描述                                       |
| ----------- | ------------------------------------------ |
| value.sync  | 接收表单的数据对象                         |
| formInfo    | 传入表单内部的数据结构对象，见``数据结构`` |
| labelSuffix | label的后缀                                |



## Form Methods（方法）

| 名称         | 描述                                                         |
| ------------ | ------------------------------------------------------------ |
| resetFields  | 对整个表单进行重置，将所有字段值重置为初始值并移除校验结果   |
| getFileList  | 获取上传的文件流，```数组```形式展示，type是upload的情况下生效 |
| downloadFile | 当type有download时候可使用此方法拿到downloadList中每个item的信息 |



## 数据结构

| 名称         | 是否必填 | 类型   | 描述                                                         |
| ------------ | -------- | ------ | ------------------------------------------------------------ |
| data         | 否       | Array  | 表单的数据源，见``表单数据源``                               |
| rules        | 否       | Array  | 表单校验对象（规则同element ui）                             |
| inline       | 否       | Bool   | ```true```每个item项以inline-block展示，```false```,每个item项独占一行 |
| labelWidth   | 否       | String | item-lable的宽度，不写label独占一行                          |
| fileTypeList | 否       | Array  | 限制上传文件的类型                                           |
| limit        | 否       | Number | 限制上传的个数                                               |



## 数据结构示例

```js
formInfo: {
        data: [
          {
            type: 'text',
            label: '名称',
            prop: 'name',
            placeholder: '请输入'
          },
          {
            type: 'select',
            label: '状态',
            prop: 'status',
            options: [
              {
                label: '是',
                value: 0
              },
              {
                label: '否',
                value: 1
              }
            ]
          },
          {
            type: 'date',
            label: '创建时间',
            prop: 'createTime'
          },
           {
            type: 'upload',
            label: '附件上传',
            prop: 'upload',
            fileTypeList: ['jpg', 'png', 'pdf'],
            limit: 3
          },
          {
            type: 'download',
            label: '处理结果',
            prop: 'dw2',
            downloadList: [ { name: val, id, fileType: 1 } ],// 在组件中动态添加，，不能写死
            disabled: true
   		 },
        ],
        rules: {
          name: [
            {
              required: true,
              message: '请填写必填项',
              trigger: 'blur'
            }
          ],
          status: [
            {
              required: true,
              message: '请填写必填项',
              trigger: 'change'
            }
          ]
        }
      },
```

# 表单数据源

## 属性

| 名称        | 描述                                         |
| ----------- | -------------------------------------------- |
| type        | item的类型，详情见``type类型``               |
| label       | el-form-item的名称                           |
| prop        | 表单域 model 字段                            |
| placeholder | 占位符文本                                   |
| width       | 每个item的宽度，inline为ture时，此属性为必须 |



## type类型

| 类型                        | 是否需要options                 | 描述                                   |
| --------------------------- | ------------------------------- | -------------------------------------- |
| text                        | 否                              | 文本框 \<el-input>                     |
| select                      | 是                              | 下拉框\<el-select>                     |
| date \| daterange\|dateTime | 否                              | 日期框\<el-picker>                     |
| upload                      | 否                              | 文件上传框（详情见upload）\<el-upload> |
| hidden                      | 否                              | 隐式的\<el-input>                      |
| download                    | 否 需要downloadLIst(详情见示例) | 提供下载的按钮                         |

## upload

| 属性         | 类型   | 说明               |
| ------------ | ------ | ------------------ |
| fileTypeList | Array  | 限制上传文件的类型 |
| limit        | Number | 限制上传的个数     |



## 


# form表单

**基于element ui的二次封装，里面的类型什么的都```和element一样```，这里就不过多赘述**

## 例子

```html
<QueryForm :formInfo="formInfo" :value.sync="dataForm" ref="queryFormRef" />
```



## 属性

| 名称       | 描述                       |
| ---------- | -------------------------- |
| value.sync | 接收表单的数据对象         |
| formInfo   | 传入表单内部的数据结构对象 |



## 方法

| 名称        | 描述                                                       |
| ----------- | ---------------------------------------------------------- |
| resetFields | 对整个表单进行重置，将所有字段值重置为初始值并移除校验结果 |



## 

## 数据结构

| 名称  | 是否必填 | 描述                             |
| ----- | -------- | -------------------------------- |
| data  | 否       | 表单对象                         |
| rules | 否       | 表单校验对象（规则同element ui） |



## 数据结构

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
          }
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

## type类型

| 类型              | 是否需要options | 描述              |
| ----------------- | --------------- | ----------------- |
| text              | 否              | 文本框 <el-input> |
| select            | 是              | 下拉框<el-select> |
| date \| daterange | 否              | 日期框<el-picker> |
|                   |                 |                   |
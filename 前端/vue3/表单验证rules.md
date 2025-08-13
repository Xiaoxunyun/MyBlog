Vue3 + Element Plus 表单验证快速入门
===

> **Tags:** vue3, element plus, 表单验证, rules

表单验证在 Vue3 + Element Plus 中很常用，尤其是配合 `rules` 配置来实现必填、选填、长度、格式等限制。

基本思路
---

1. 使用 `el-form` 包裹表单项
2. 用 `:model` 绑定数据对象
3. 用 `:rules` 绑定验证规则
4. 每个 `el-form-item` 的 `prop` 要与规则中的 key 对应

------

例子：名字必填，描述选填
---

```vue
<template>
  <el-form
    ref="formRef"
    :model="form"
    :rules="rules"
    label-width="80px"
  >
    <!-- 名字 必填 -->
    <el-form-item label="名字" prop="name">
      <el-input v-model="form.name" placeholder="请输入名字" />
    </el-form-item>

    <!-- 描述 选填 -->
    <el-form-item label="描述" prop="desc">
      <el-input v-model="form.desc" placeholder="请输入描述（可选）" />
    </el-form-item>

    <el-form-item>
      <el-button type="primary" @click="submitForm">提交</el-button>
      <el-button @click="resetForm">重置</el-button>
    </el-form-item>
  </el-form>
</template>

<script setup>
import { ref } from 'vue'

const formRef = ref(null)
const form = ref({
  name: '',
  desc: ''
})

const rules = {
  name: [
    { required: true, message: '名字不能为空', trigger: 'blur' }
  ],
  desc: [
    { min: 0, max: 50, message: '描述不能超过50个字符', trigger: 'blur' }
  ]
}

const submitForm = () => {
  formRef.value.validate((valid) => {
    if (valid) {
      console.log('验证通过', form.value)
    } else {
      console.log('验证失败')
    }
  })
}

const resetForm = () => {
  formRef.value.resetFields()
}
</script>
```

------

常用验证写法
---

### 1. 必填字段

```js
{ required: true, message: '请输入内容', trigger: 'blur' }
```

### 2. 长度限制

```js
{ min: 3, max: 10, message: '长度在3-10个字符之间', trigger: 'blur' }
```

### 3. 正则验证（如邮箱）

```js
{ 
  pattern: /^[^\s@]+@[^\s@]+\.[^\s@]+$/, 
  message: '邮箱格式不正确', 
  trigger: 'blur' 
}
```

------

小结
---

- **必填**：`required: true`
- **选填**：不写 `required`，但可以加其他限制
- **`prop` 名必须和 rules 对应**
- 触发方式常用 `blur`（失焦）和 `change`（值变化）
# Element-ui-plus(二次封装)

## 一，form表单的二次封装

### 1，组件代码

```vue
<template>
  <div>
    <slot name="header"></slot>
  </div>
  <el-form
      ref="ruleFormRef"
      :label-width="labelWidth"
      :model="modelValue"
  >
    <!--      通过数据配置生成表单   -->
    <template v-for="(item,index) in formItem" :key="index">
      <el-form-item :label="item.label" :prop="item.model">
        <template v-if="item.type === 'input' || item.type === 'password'">
          <el-input
              v-model="modelValue[item.model]"
              :placeholder="item.placeholder"
              :show-password="item.type === 'password'"
          />
        </template>
        <template v-else-if="item.type === 'switch'">
          <el-switch v-model="modelValue[item.model]"/>
        </template>
        <template v-else-if="item.type === 'select'">
          <el-select v-model="modelValue[item.model]" :placeholder="item.placeholder">
            <el-option
                v-for="option in item.options"
                :key="option.value"
                :label="option.label"
                :value="option.value"
            />
          </el-select>
        </template>
        <template v-else-if="item.type === 'date'">
          <el-date-picker
              :type="item.type"
              :placeholder="item.placeholder"
              v-model="modelValue[item.model]"
          />
        </template>
      </el-form-item>
    </template>
  </el-form>
  <div>
    <slot name="footer"></slot>
  </div>
</template>

<script setup>
import {defineProps, defineExpose, ref} from 'vue'

defineProps({
  formItem: {
    type: Array,
  },
  labelWidth: {
    type: String,
    default: '120px'
  },
  modelValue: {
    type: Object
  }
})
const ruleFormRef = ref()
/**
 * reset
 */
const resetForm = () => {
  ruleFormRef.value?.resetFields()
}
/**
 * 父组件访问
 */
defineExpose({
  resetForm
})
</script>

<style scoped lang="scss">

</style>

```

### 2，使用方式

```vue
<script setup>
/**
 *数据单独存放，具体看个人习惯
 */
import FormText from "@/utils/formTest.js"
import {reactive, ref} from "vue";

let formRef = ref()
const form = reactive({})
const fromData = FormText.formItem ?? []
fromData.map(item => {
  form[item.model] = ''
})
const submit = () => {
  console.log(form)
}
/**
 * 使用子组件
 */
const cancel = () => {
  console.log('cancel')
  formRef.value?.resetForm()
}
</script>

<template>
  <div>
    <!--  v-bind 一对多传值 :属性="属性" 一对一传值-->
    <form-input
        v-bind="FormText"
        v-model="form"
        ref="formRef"
    >
    </form-input>
  </div>
</template>

<style scoped lang="scss">

</style>

```

### 3，数据使用

```js
export default {
    formItem: [
         {
            type: 'selectType',
            label: 'selectLabel',
            model:'selectModel',
            placeholder: 'you select',
            options: [
                {
                    label: 'red',
                    value: '0'
                },
                {
                    label: 'yellow',
                    value: '1'
                }
            ]
        },
    ],
    labelWidth: '120px'
}

```

(只说对象属性字段)

|    字段     |  类型  |        种类        | 可选类型                                     |
| :---------: | :----: | :----------------: | :------------------------------------------- |
|    type     | string |       输入框       | input,password,switch,select,date            |
|    model    | string |    数据双向绑定    | 无                                           |
| placeholder | string |   输入框提示信息   | 无                                           |
|    label    | string |        标题        | 无                                           |
|   options   | Array  | 仅select的必填字段 | label（显示选择字段），value（字段对应的值） |
| labelWidth  | string |      表格宽度      | 默认120px                                    |


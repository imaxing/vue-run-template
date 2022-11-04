# 实现 vue 组件的实时修改实时运行

> 类似 ElementUi 文档的组件示例效果并可实时调试

---

**安装**

```bash
npm install vue-complier-template
```

**用法**

```js
// main.js
import Vue from "vue";
import vueComplierTemplate from "vue-complier-template";
Vue.use(vueComplierTemplate, {
  parseStyles: ({ styles, vm }) => {},
  evalScript: (value) => window.eval(value),
  render: ({ h, descriptor }) => {},
  renderError: ({ h, descriptor, error }) => {},
  renderEmpty: ({ h, descriptor }) => {},
});
```

```js
// example-source-code.js
export default `<script>
  export default {
    data() {
      return {
        tableData: Array(6).fill('').map((_, index) => ({
          date: '2016-05-02',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1519弄'
        }))
      }
    }
  }
</script>

<template>
  <el-table :data="tableData" border>
    <el-table-column prop="date" label="日期" width="180"/>
    <el-table-column prop="name" label="姓名" width="180"/>
    <el-table-column prop="address" label="地址" />
  </el-table>
</template>
`;
```

```html
<!-- example.vue -->
<template>
  <div id="app">
    <vue-complier-template v-model="codeValue" />
    <textarea v-model="codeValue" />
  </div>
</template>

<script>
  import codeValue from "path/to/example-source-code.js";
  export default {
    name: "App",
    data: () => ({ codeValue }),
  };
</script>
```

**Options**

| 参数        | 说明                                                        | 类型                               | 可选值 | 默认值 |
| ----------- | ----------------------------------------------------------- | ---------------------------------- | ------ | ------ |
| value       | value / v-model 绑定值                                      | string                             | -      | -      |
| render      | 接管组件内部 render                                         | Function({ h, descriptor })        | -      | -      |
| renderError | value 解析失败内容渲染函数                                  | Function({ h, error, descriptor }) | -      | -      |
| renderEmpty | value 为空时内容渲染函数                                    | Function({ h, descriptor })        | -      | -      |
| evalScript  | 解析 script 字符串(必填)                                    | Function(value)                    | -      | -      |
| parseStyles | value 解析出的 style 标签内的内容, 抛出数据调用方自定义处理 | Function({ styles, vm })           | -      | -      |

**DEMO**

[![Edit vue-complier-template-example](https://codesandbox.io/static/img/play-codesandbox.svg)](https://codesandbox.io/s/vue-complier-template-example-4qjo20?fontsize=14&hidenavigation=1&theme=dark)

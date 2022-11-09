# virtual-list

#### 简单的虚拟列表实现


| 属性          | 说明          | 默认值 | 类型            |
|-------------|-------------|-----|---------------|
| list        | 列表数据数组      | []  | Array[Object] |
| height      | 列表高度        | 0   | Number        |
| itemHeight  | 单个item高度    | 0   | Number        |
| splitCol    | 每行几个，默认一行一个 | 1   | Number        |
| renderCount | 实际渲染的dom节点数 | 10  | Number        |

```html
<VirtualList :list="list" :height="300" :item-height="40" :split-col="3">
  <template #default="{ item }">
    {{ item }}
  </template>
</VirtualList>
```

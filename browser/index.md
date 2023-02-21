# 强缓存和协商缓存
哈才

# Vue + ElementUI 项目

?> 我在平时项目中遇到的比较**有趣的需求**, 特意收集在下面, 或许有人可能也会遇到同样的场景, 可直接复制我的代码进行二次封装

<br />

### 二次开发 ElementUI 组件

```vue [MyInput.vue] { ...vueConfig }
<template>
  <my-input
    v-model="name"
    placeholder="请输入"
    @input="handleInput"
  >
    <template slot="prepend">http://</template>
    <template slot="append">.com</template>
  </my-input>
</template>

<script>
// 子组件
const MyInput =  {
  template: `
    <el-input
      v-bind="$attrs"
      v-on="$listeners"
    >
      <template v-for="(index, name) in $slots" :slot="name">
        <slot :name="name" />
      </template>
    </el-input>
  `,
}
// 父组件
export default {
  components: {
    'my-input': MyInput
  },
  data () {
    return {
      name: 'baidu'
    }
  },
  methods: {
    handleInput (v) {
      console.log(v)
    }
  }
}
</script>
```

### 下拉框 + 搜索过滤 + 关键词标红

?> 常见的搜索功能, 当输入内容时, 下拉框里所有匹配的选项会**`自动过滤`**并**`标红显示`**

```vue [App.vue] { ...vueConfig }
<template>
  <div>
    <el-select
      v-model="value"
      filterable
      placeholder="请选择"
      :filter-method="handleFilter"
      @visible-change="v => !v && handleFilter('')"
    >
      <el-option
        v-for="item in options"
        :key="item.value"
        :label="item.label"
        :value="item.value"
      >
        <span
          v-for="(s, i) in item.label"
          :key="i"
          :style="{ color: isRed(item.label, i) ? 'red' : '0' }"
        >{{s}}</span>
      </el-option>
    </el-select>
  </div>
</template>

<script>
const options = [
  { value: 1, label: '北京市' },
  { value: 2, label: '北京烤鸭' },
  { value: 3, label: '北京市南大街' },
  { value: 4, label: '北京市北大街' },
  { value: 5, label: '北京市大街店铺' },
]
export default {
  data () {
    return {
      value: '',
      search: '',
      options,
    }
  },
  methods: {
    isRed (str, i) {
      const { search } = this
      const index = str.indexOf(search)
      return index <= i && i < search.length + index
    },
    handleFilter (val) {
      this.search = val
      this.options = options.filter(item => {
        return item.label.indexOf(val) > -1
      })
    }
  }
}
</script>
```

<br />

### 下拉框 - 清空全部数据前二次确认

?> 针对多选下拉框进行优化, 在清空所有数据前, 进行弹窗询问, `二次确认后再清空所有数据`

```vue [App.vue] { ...vueConfig }
<template>
  <el-select
    v-model="value"
    :multiple="true"
    :clearable="true"
    placeholder="请选择"
    @clear="handleClear"
  >
    <el-option
      v-for="item in options"
      :key="item.value"
      :label="item.label"
      :value="item.value"
    ></el-option>
  </el-select>
</template>

<script>
export default {
  data() {
    return {
      value: [1, 2, 3, 4],
      clearResolve: null,
      options: [
        { value: 1, label: '黄金糕' },
        { value: 2, label: '双皮奶' },
        { value: 3, label: '蚵仔煎' },
        { value: 4, label: '龙须面' },
        { value: 5, label: '北京烤鸭' },
      ],
    }
  },
  watch: {
    value (newVal, oldVal) {
      const { clearResolve } = this
      if (typeof clearResolve === 'function') {
        clearResolve(oldVal)
      }
    }
  },
  methods: {
    async handleClear () {
      const value = await new Promise(resolve => {
        this.clearResolve = resolve
      })
      this.clearResolve = null
      this.value = value
      this.$confirm('是否清除所有选中的值?', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(() => {
        this.value = []
      }).catch(() => {})
    }
  }
}
</script>
```

<br />

### 级联选择器 - 清空全部数据前二次确认

?> 由于 **&lt;el-cascader />** 组件没有提供 clear 回调, 只能借助于其他方式模拟, 才能实现 `二次确认后再清空所有数据` 的需求

```vue [App.vue] { ...vueConfig }
<template>
  <el-cascader
    v-model="value"
    :options="options"
    :clearable="true"
    :props="{ multiple: true, emitPath: true }"
    @remove-tag="handleRemove"
  ></el-cascader>
</template>

<script>
export default {
  data () {
    return {
      value: [[1], [2], [3], [4]],
      options: [
        { value: 1, label: '北京' },
        { value: 2, label: '上海' },
        { value: 3, label: '广州' },
        { value: 4, label: '深圳' },
      ],
      isRemoveTag: false,
      isCleaning: false,
    }
  },
  watch: {
    value (newVal, oldVal) {
      if (!this.isRemoveTag && !newVal.length) {
        this.handleClear(oldVal)
      }
      this.isRemoveTag = false
    }
  },
  methods: {
    handleClear (value) {
      if (this.isCleaning) return
      this.isCleaning = true
      this.value = JSON.parse(JSON.stringify(value))
      this.$confirm('是否清除所有选中的值?', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(() => {
        this.value = []
      }, () => {}).finally(() => {
        this.isCleaning = false
      })
    },
    handleRemove () {
      this.isRemoveTag = true
    }
  }
}
</script>
```
```vue [MyCascader.vue]
<template>
  <el-cascader v-bind="$attrs" v-on="$listeners" @remove-tag="handleRemove">
    <template v-for="(index, name) in $slots" :slot="name">
      <slot :name="name"></slot>
    </template>
  </el-cascader>
</template>

<script>
export default {
  name: 'MyCascader',
  data () {
    return {
      isRemoveTag: false,
      isCleaning: false
    }
  },
  watch: {
    '$attrs.value': function (newVal, oldVal) {
      if (!this.isRemoveTag && !newVal.length) {
        this.handleClear(oldVal)
      }
      this.isRemoveTag = false
    }
  },
  methods: {
    handleClear (value) {
      if (this.isCleaning) return
      this.isCleaning = true
      this.$nextTick(() => {
        this.$emit('input', JSON.parse(JSON.stringify(value)))
      })
      this.$confirm('是否清除所有选中的值?', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(() => {
        this.$emit('input', [])
      }, () => {}).finally(() => {
        this.isCleaning = false
      })
    },
    handleRemove () {
      this.isRemoveTag = true
    }
  }
}
</script>
```
### 1.前言
<script setup>是在单文件组件中使用Composition API的编译时语法糖。相比于普通的<script>语法，它具有更多优势:  
1.更少的样板内容，更简洁的代码
2.能够使用纯 Typescript 声明 props 和抛出事件
3.更好的运行时性能 (其模板会被编译成与其同一作用域的渲染函数，没有任何的中间代理)
4.更好的 IDE 类型推断性能 (减少语言服务器从代码中抽离类型的工作)

### 2.基本语法 

```
<template>
  <p>{{ name }}</p>
</template>

<script setup lang="ts">
    let name = '小明'
</script>  
```

script里面的代码会被编译成组件setup()函数的内容。这意味着与普通的<script>只在组件被首次引入的时候执行一次不同，<script setup>中的代码会在每次组件实例被创建的时候执行。
注意：当使用<script setup>的时候，任何在<script setup>声明的顶层的绑定 (包括变量，函数，以及import引入的内容) 都能在模板中直接使用，不需要return

响应式
响应式状态需要使用响应式APIs来创建

```  
<template>
  <p>{{ name }}</p>
  <p>{{ data.title }}</p>
</template>

<script setup lang="ts">
  import { ref, reactive } from 'vue'

  let name = ref('小明')
  let data = reactive({
    title: '标题'
  })
</script>

### 3.组件使用
<script setup>范围里的值也能被直接作为自定义组件的标签名使用，不需要写在conmonent对象里
```
<template>
  <MyComponent />
</template>

<script setup>
    import MyComponent from './MyComponent.vue'
</script>
```

3.1动态组件
由于组件被引用为变量而不是作为字符串键来注册的，在<script setup>中要使用动态组件的时候，就应该使用动态的:is来绑定
```
<template>
  <component :is="Foo" />
  <component :is="someCondition ? Foo : Bar" />
</template>

<script setup>
    import Foo from './Foo.vue'
    import Bar from './Bar.vue'
</script>
```
3.2递归组件
一个单文件组件可以通过它的文件名被其自己所引用。例如：文件名为Foo.vue的组件可以在其模板中用<Foo/>引用它自己。如果名称冲突了，就需要使用别名。
```
import { Foo as FooBarChild } from './components'
```
### 4.自定义指令
全局注册的自定义指令将以符合预期的方式工作，且本地注册的指令可以直接在模板中使用，就像上文所提及的组件一样。但这里有一个需要注意的限制：必须以 vNameOfDirective的形式来命名本地自定义指令，以使得它们可以直接在模板中使用

基本语法
```
<template>
  <h1 v-my-directive>This is a Heading</h1>
</template>

<script setup>
    const vMyDirective = {
      beforeMount: (el) => {
        // 在元素上做些操作
      }
    }
</script>
```
导入指令
```
<script setup>
  // 导入的指令同样能够工作，并且能够通过重命名来使其符合命名规范
  import { myDirective as vMyDirective } from './MyDirective.js'
</script>
```
### 5.props
在<script setup>中必须使用defineProps来声明props，它具备完整的类型推断并且在<script setup>中是直接可用的
```
<script setup>
    const props = defineProps({
      foo: String
    })
</script>
```
5.1.TypeScript支持
仅限类型声明

```
const props = defineProps<{
  foo: string
  bar?: number
}>()
```

默认值
```
interface Props {
  msg?: string
  labels?: string[]
}

const props = withDefaults(defineProps<Props>(), {
  msg: 'hello',
  labels: () => ['one', 'two']
})
```
### 6.emit
在<script setup>中必须使用defineEmits来声明emits，它具备完整的类型推断并且在<script setup>中是直接可用的
```
<script setup>
    const emit = defineEmits(['change', 'delete'])
</script>
```
#### 6.1.TypeScript支持
仅限类型声明
```
const emit = defineEmits<{
  (e: 'change', id: number): void
  (e: 'update', value: string): void
}>()
```
### 7.defineExpose暴露
使用<script setup>的组件是默认关闭的，也即通过模板ref或者$parent链获取到的组件的公开实例，不会暴露任何在<script setup>中声明的绑定
```
<script setup>
    import { ref } from 'vue'
    
    const a = 1
    const b = ref(2)
    
    defineExpose({
      a,
      b
    })
</script>
```
### 8.useSlots 和 useAttrs
在模板中通过$slots和$attrs来访问它们
```
<script setup>
    import { useSlots, useAttrs } from 'vue'
    
    const slots = useSlots()
    const attrs = useAttrs()
</script>
```
### 9.与普通的script一起使用
<script setup>可以和普通的<script>一起使用。普通的<script>在有这些需要的情况下或许会被使用到。

无法在<script setup>声明的选项，例如inheritAttrs或通过插件启用的自定义的选
声明命名导出
运行副作用或者创建只需要执行一次的对象
```
<script>
    // 普通 <script>, 在模块范围下执行(只执行一次)
    runSideEffectOnce()
    
    // 声明额外的选项
    export default {
      inheritAttrs: false,
      customOptions: {}
    }
</script>

<script setup>
    // 在 setup() 作用域中执行 (对每个实例皆如此)
</script>
```
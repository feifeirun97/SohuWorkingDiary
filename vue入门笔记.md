# Vue2

### 特点

+ 数据绑定

  改变input标签的值将**自动同步**更新到其他页面上绑定该输入框的组件

+ 组件化

  小至一个按钮都可以是一个单独的文件`.vue`

### 开发环境

+ #### Homebrew

+ Node.js

+ Npm

+ Webpack

  Vue组件都是通过`.vue`或者如微信小程序的`.wxml`和`.wxss`等自定义组件都无法被用户端各种浏览器解析，需要被翻译和打包成`.js`文件

+ Vue-cli

  用来生成模版的vue工程，相当于按照设计好的图来盖房子。封装了脚手架

### 创建项目

```zsh
#创建项目
vue init webpack-simple 工程名字<工程名字不能用中文>
#安装
npm install vue-router vue-resource --save 
```

### 规范

+ `App.vue`文件下`<template>`内写html
+ `App.vue`文件下`<script>`内写js
+ `App.vue`文件下`<style>`内写html
+ `<template>`内有且只有一个`<div>`
+ 数据要写在`data(){return {}}`内

### 简单页面

```vue
<!--小组件-->
<template>
  <div id="firstcomponent">
    <h1>I am a title.</h1>
    <a> written by {{ author }} </a>
  </div>
</template>

<script type="text/javascript">
export default {
  data() { return { author: "微信公众号 jinkey-love" }; }
};
</script>

<!--App.vue-->
<template>
  <div id="app">
    <img src="./assets/logo.png" />
    <h1>{{ msg }}</h1>
    <fistComponent></fistComponent>
  </div>
</template>

<script>
import fistComponent from "./component/firstComponent.vue";
export default {
  data() {
    return {
      msg: "Hello Vu-e!"
    };
  },
  components: { fistComponent }
};
</script>
```

### 路由搭建

在`webpack.config.js`加入别名`resolve:{ vue: "vue/dist/vue.js"}`。这里原因涉及到两种构建方式:

+ **独立构建**

  包括编译和支持`template`选项。它也依赖于浏览器接口的存在所以不能用它来为服务器端渲染

+ **运行时构建**

  不包括模版编译,不支持`template`选项。运行时构建, 可以用`render`选项, 但它只在单文件的组件起作用, 因为单文件组件的模版是在构建时预编译到`render`函数中, 运行时构建只有独立构建大小的30%, 只有16b到min+gzip大小

默认`NPM`包导出的是运行时构建。这里是为了使用独立构建



# Vue2基础

### Vue实例

每一个应用都是通过`Vue`函数创建一个新的**Vue实例**。Vue的设计很大程度参考了MVVM模型, 所以经常会用`vm` [viewmodel]来表示Vue实例。

### 数据

当一个 Vue 实例被创建时，它将 `data` 对象中的所有的 property 加入到 Vue 的**响应式系统**中。当这些 property 的值发生改变时，视图将会产生“响应”，即匹配更新为新的值。

当`data`里的数据改变时回触发重渲染。但只有实例被创建时就已存在于`data`的才是**响应式**的，后续添加的无效

```js
// 我们的数据对象
var data = { a: 1 }

// 该对象被加入到一个 Vue 实例中
var vm = new Vue({
  data: data
})
vm.b = 10 //无效

// 获得这个实例上的 property
// 返回源数据中对应的字段
vm.a == data.a // => true

// 设置 property 也会影响到原始数据
vm.a = 2
data.a // => 2

// ……反之亦然
data.a = 3
vm.a // => 3
```

### 方法

`$`开头的方法是Vue实例自带的方法，为了与用户自定义的`property`区分开来

```js
var data = { a: 1 }
var vm = new Vue({
  el: '#example',
  data: data
})

vm.$data === data // => true
vm.$el === document.getElementById('example') // => true

// $watch 是一个实例方法
vm.$watch('a', function (newValue, oldValue) {
  // 这个回调将在 `vm.a` 改变后调用
})
```

### 实例生命周期钩子

每个实例在被创建的时候都需要一系列初始化--设置数据监听，编译模版，将实例挂载在DOM并在数据变化时更新DOM等。在这个过程中会运行一些**生命周期钩子**端函数，用户可以在不同阶段进行副作用操作。

常见钩子如 mounted、`updated`和 `destroyed`。生命周期钩子的 `this` 上下文指向调用它的 Vue 实例。⚠️箭头函数拿不到property

```js
//created钩子可以用在实例被创建后执行代码
new Vue({
  data: {
    a: 1
  },
  created: function () {
    // `this` 指向 vm 实例。
    console.log('a is: ' + this.a)
  }
})
// => "a is: 1"
```

![](https://s2.loli.net/2022/07/27/t1ZWuYeFIwhSqUJ.png)

### 模版语法

Vue.js 使用了基于 HTML 的模板语法，允许开发者声明式地将 DOM 绑定至底层 Vue 实例的数据。所有 Vue.js 的模板都是合法的 HTML，所以能被遵循规范的浏览器和 HTML 解析器解析。

在底层的实现上，Vue 将模板编译成虚拟 DOM 渲染函数。结合响应系统，Vue 能够智能地计算出最少需要重新渲染多少组件，并把 DOM 操作次数减到最少。

### 插值

#### 文本

数据绑定使用`Mustache`语法即`{{msg}}`进行插值。`v-once`指令可以执行一次性插值 [数据改变插值处不更新]

```vue
<span v-once>不改变{{ msg }}</span>
```

#### 原始HTML

双大括号会将数据解释为普通文本而非html代码，此时需要`v-html`指令。会在当前`html`标签下新建一个`rawHtml`

```vue
<template>
  <div id="app">
    <p>{{ rawHtml }}</p>
    <span v-html="rawHtml"></span>
  </div>
</template>

<script type="text/javascript">
export default { data() { return { rawHtml: '<span style="color:red">Hello</span>' }; }};
</script>
```

#### Attribute

双大括号不能作用在`html`元素上需要`v-bind`指令。可以绑定一些类名或id等

```html
<template>
  <div id="app">
    <div v-bind:class="color">{{ rawHtml }}</div>
    <div v-bind:id='dynamicId'></div>
    <span v-html="rawHtml"></span>
  </div>
</template>

<script type="text/javascript">
export default {
  data() {
    return {
      rawHtml: '<span style="color:red">Hello</span>',
      color: "myFont"
    };
  }
};
</script>

<style>
.myFont { color: orange; font-weight: bold; }
</style>
```

#### JS表达式

js语句不生效，单个表达式生效ˉ

```jsx
{{ number + 1 }}
{{ ok ? 'YES' : 'NO' }}
{{ message.split('').reverse().join('') }}
<div v-bind:id="'list-' + id"></div>
<!-- 这是语句，不是表达式 -->
{{ var a = 1 }}
<!-- 流控制也不会生效，请使用三元表达式 -->
{{ if (ok) { return message } }}
```


# Vue2

### 特点

+ 数据绑定

  改变input标签的值将**自动同步**更新到其他页面上绑定该输入框的组件

+ 组件化

  小至一个按钮都可以是一个单独的文件`.vue`

### 开发环境

+ Homebrew

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

## Vue实例

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

常见钩子如 `mounted`、`updated`和 `destroyed`。生命周期钩子的 `this` 上下文指向调用它的 Vue 实例。⚠️箭头函数拿不到property

```js
//created钩子可以用在实例被创建后执行代码
new Vue({
  data: {
    a: 1
  },
  created: function () {
    console.log('a is: ' + this.a)
  }
})
// => "a is: 1"
```

<img src="https://s2.loli.net/2022/07/27/t1ZWuYeFIwhSqUJ.png" style="zoom:50%;" />

## 模版语法

Vue.js 使用了基于 HTML 的模板语法，允许开发者声明式地将 DOM 绑定至底层 Vue 实例的数据。所有 Vue.js 的模板都是合法的 HTML，所以能被遵循规范的浏览器和 HTML 解析器解析。

在底层的实现上，Vue 将模板编译成虚拟 DOM 渲染函数。结合响应系统，Vue 能够智能地计算出最少需要重新渲染多少组件，并把 DOM 操作次数减到最少。

### 插值

#### 文本

数据绑定使用`Mustache双大括号`语法即`{{msg}}`进行插值。`v-once`指令可以执行一次性插值 [数据改变插值处不更新]

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

双大括号不能作用在`html`元素上。`v-bind`指令可以绑定一些类名或id等来解决问题

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

js语句不生效，仅单个表达式生效

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

### 指令

指令 (Directives) 是带有 `v-` 前缀的特殊 attribute。指令 attribute 的值预期是**单个 JavaScript 表达式** (`v-for` 是例外情况，稍后我们再讨论)。指令的职责是，当表达式的值改变时，将其产生的连带影响，响应式地作用于 DOM。回顾我们在介绍中看到的例子：

```html
<p v-if="seen">现在你看到我了</p>
```

这里，`v-if` 指令将根据表达式 `seen` 的值的真假来插入/移除 `<p>` 元素。

#### 参数

一些指令能够接收一个“参数”，在指令名称之后以冒号表示。例如，`v-bind` 指令可以用于响应式地更新 HTML attribute：

```html
<a v-bind:href="url">...</a>
```

在这里 `href` 是参数，告知 `v-bind` 指令将该元素的 `href` attribute 与表达式 `url` 的值绑定。

另一个例子是 `v-on` 指令，它用于监听 DOM 事件：

```html
<a v-on:click="doSomething">...</a>
```

在这里参数是监听的事件名。我们也会更详细地讨论事件处理

#### 动态参数

```html
<!--注意，参数表达式的写法存在一些约束，如之后的“对动态参数表达式的约束”章节所述。-->
<a v-bind:[attributeName]="url"> ... </a>
```

这里的 `attributeName` 会被作为一个 JavaScript 表达式进行动态求值，求得的值将会作为最终的参数来使用。例如，如果你的 Vue 实例有一个 `data` property `attributeName`，其值为 `"href"`，那么这个绑定将等价于 `v-bind:href`。

同样地，你可以使用动态参数为一个动态的事件名绑定处理函数：

```
<a v-on:[eventName]="doSomething"> ... </a>
```

在这个示例中，当 `eventName` 的值为 `"focus"` 时，`v-on:[eventName]` 将等价于 `v-on:focus`。

#### 对动态参数的值的约束

动态参数预期会求出一个字符串，异常情况下值为 `null`。这个特殊的 `null` 值可以被显性地用于移除绑定。任何其它非字符串类型的值都将会触发一个警告。

#### 对动态参数表达式的约束

动态参数表达式有一些语法约束，因为某些字符，如空格和引号，放在 HTML attribute 名里是无效的。例如：

```html
<!-- 这会触发一个编译警告 -->
<a v-bind:['foo' + bar]="value"> ... </a>
```

变通的办法是使用没有空格或引号的表达式，或用计算属性替代这种复杂表达式。

在 DOM 中使用模板时 (直接在一个 HTML 文件里撰写模板)，还需要避免使用大写字符来命名键名，因为**浏览器会把 attribute 名全部强制转为小写**：

```html
<!--
在 DOM 中使用模板时这段代码会被转换为 `v-bind:[someattr]`。
除非在实例中有一个名为“someattr”的 property，否则代码不会工作。
-->
<a v-bind:[someAttr]="value"> ... </a>
```

#### 修饰符

修饰符 (modifier) 是以 `.` 后缀指出指令应该以特殊方式绑定。如下`.prevent` 告诉 `v-on` 指令对于触发的事件调用 `event.preventDefault()`：

```
<form v-on:submit.prevent="onSubmit">...</form>
```

在接下来对 [`v-on`](https://v2.cn.vuejs.org/v2/guide/events.html#事件修饰符) 和 [`v-for`](https://v2.cn.vuejs.org/v2/guide/forms.html#修饰符) 等功能的探索中，你会看到修饰符的其它例子。

### 缩写

 `v-bind` 和 `v-on` 这两个最常用的指令，提供了特定简写：

#### v-bind缩写

```html
<!-- 完整语法 -->
<a v-bind:href="url">...</a>

<!-- 缩写 -->
<a :href="url">...</a>

<!-- 动态参数的缩写 (2.6.0+) -->
<a :[key]="url"> ... </a>
```

#### v-on缩写

```html
<!-- 完整语法 -->
<a v-on:click="doSomething">...</a>

<!-- 缩写 -->
<a @click="doSomething">...</a>

<!-- 动态参数的缩写 (2.6.0+) -->
<a @[event]="doSomething"> ... </a>
```

## 计算属性和侦听器

### 计算属性

模版内表达式应尽量简洁，` {{ message.split('').reverse().join('') }}`明显为复杂逻辑，应使用**计算属性**[无副作用]

```vue
<template>
  <div id='example'>
    <p>Original message: "{{ message }}"</p>
 		<p>Computed reversed message: "{{ reversedMessage }}"</p>
  </div>
</template>

<script>
  var vm = new Vue({
    el: '#example',
    data: { message: 'Hello' },
    computed: { reversedMessage: function ()  return this.message.split('').reverse().join('') }
})
</script>
```

#### 计算属性缓存vs方法

```vue
<template>
  <div id='example'>
    <p>Computed reversed message: "{{ reversedMessage() }}"</p>
  </div>
</template>

<script>
  var vm = new Vue({
    el: '#example',
    data: { message: 'Hello' },
    methods: { reversedMessage: function () return this.message.split('').reverse().join('') }
	})
</script>
```

很明显我们可以用方法代替计算属性, 那么**区别**是什么呢？

+ **计算属性是基于它们的响应式依赖进行缓存的**。只在相关响应式依赖发生改变时它们才会重新求值，这就意味着只要 `message` 还没有发生改变，多次访问 `reversedMessage` 计算属性会立即返回之前的计算结果，而不必再次执行函数。
+ **调用方法触发重新渲染时总会再次执行**

#### 计算属性vs侦听属性

**侦听属性**可以观察和响应 Vue 实例上的数据变动。当有很多数据需要随其它数据变动时，很容易滥用 `watch`，更好的做法是使用计算属性而不是命令式的 `watch` 回调。细想一下这个例子：

```vue
<template>
 <div id="demo">{{ fullName }}</div>
</template>

<script>
  var vm = new Vue({
    el: '#demo',
    data: { firstName: 'Foo', lastName: 'Bar', fullName: 'Foo Bar' },
    //用watch监听改变fullName的状态
    watch: {
      firstName: function (val) { this.fullName = val + ' ' + this.lastName },
      lastName: function (val) { this.fullName = this.firstName + ' ' + val }
 	 	},
    //用computed简洁很多
    computed: {
    	fullName: function () { return this.firstName + ' ' + this.lastName }
  }
	})
</script>
```

#### 计算属性的getter和setter

计算属性默认只有`getter`，不过在需要时你也可以提供一个`setter`更新其他值

现在运行 `vm.fullName = 'John Doe'`时，setter 会被调用，`vm.firstName` 和 `vm.lastName` 也会相应地被更新。

```js
computed: {
  fullName: {
    // getter
    get: function () {
      return this.firstName + ' ' + this.lastName
    },
    // setter
    set: function (newValue) {
      var names = newValue.split(' ')
      this.firstName = names[0]
      this.lastName = names[names.length - 1]
    }
  }
}
```

### 侦听器

即使计算属性适合大多数情况，又是也需要自定义的侦听器`watch`执行异步或者开销大的操作。

```vue
<template>
	<div id='watch-example'>
    <p>
      Ask a yes/no question:
      <input v-model='question'>
  	</p>
    <p>{{answer}}</p>
  </div>
</template>

<script>
  var watchExampleVM = new Vue({
    el: '#watch-example',
    data: {
      question: '',
      answer: 'Ask sth!'
    },
    watch: {
      question: function (newQ, oldQ) {
        this.answer = 'Waiting for you to stop typing...'
        this.debouncedGetAnswer()
      }
    },
    //挂载生命周期
    created: function () {
      this.debouncedGetAnswer = _.debounce(this.getAnswer, 300)
		},
    methods: {
      getAnswer: function () {
        if (this.question.indexOf('?') === -1) {
          this.answer = 'Questions usually contain a question mark. ;-)'
        	return
        }
         this.answer = 'Thinking...'
     		 var vm = this
         axios.get('https://yesno.wtf/api')
          .then(function (response) {
            vm.answer = _.capitalize(response.data.answer)
          })
          .catch(function (error) {
            vm.answer = 'Error! Could not reach the API. ' + error
          })
    }
  }
    
  })
</script>
```

## Class与Style绑定

### 绑定HTML Class

在将 `v-bind` 用于 `class` 和 `style` 时，表达式结果的类型除了字符串之外，还可以是对象或数组。

#### 对象语法

我们可以传给 `v-bind:class` 一个对象，以动态地切换 class

```vue
//Example1
<div
  class="static"
  v-bind:class="{ active: isActive, 'text-danger': hasError }"
></div>
//数据
data: {
  isActive: true,
  hasError: true
}
//结果
<div class="static active text-danger"></div>

//Example2
<div v-bind:class="classObject"></div>
data: {
  classObject: {
    active: true,
    'text-danger': false
  }
}

//Example3
<div v-bind:class="classObject"></div>
data: {
  isActive: true,
  error: null
},
computed: {
  classObject: function () {
    return {
      active: this.isActive && !this.error,
      'text-danger': this.error && this.error.type === 'fatal'
    }
  }
}
```

#### 数组语法

我们可以把一个数组传给 `v-bind:class`，以应用一个 class 列表

```vue
//Example1
<div v-bind:class="[activeClass, errorClass]"></div>
<div v-bind:class="[{ active: isActive }, errorClass]"></div>
//数据
data: {
  activeClass: 'active',
  errorClass: 'text-danger'
}
//结果
<div class="active text-danger"></div>
```

### 绑定Style

#### 对象语法

```vue
<div v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>
data: {
  activeColor: 'red',
  fontSize: 30
}

<div v-bind:style="styleObject"></div>
data: {
  styleObject: {
    color: 'red',
    fontSize: '13px'
  }
}
```

#### 数组语法

```vue
<div v-bind:style="[baseStyles, overridingStyles]"></div>
```

## 条件渲染

### v-if

#### 例子

`v-else` 元素必须紧跟在带 `v-if` 或者 `v-else-if` 的元素的后面，否则它将不会被识别。

⚠️不推荐与`v-for`一起使用因为`v-for`优先级更高

```vue
<div v-if="type === 'A'"> A </div>
<div v-else-if="type === 'B'"> B </div>
<div v-else> Not A/B/ </div>
```

#### 用key管理可复用元素

这里input输入的内容不会变，因为vdom认为元素节点无变化[与react不同]

可以加一个key来强制重渲染

```vue
<template v-if="loginType === 'username'">
  <label>Username</label>
  <input placeholder="Enter your username">
</template>
<template v-else>
  <label>Email</label>
  <input placeholder="Enter your email address">
</template>
```

### v-show

始终在DOM中，仅控制CSS-Display

```vue
<h1 v-show="ok">Hello!</h1>
```

## 列表渲染

### v-for

`v-for` 指令需要使用 `item in items` 语法， `items` 源数据数组， `item` 被迭代的数组元素的**别名**。

#### 使用

+ `v-for="(item, index) in items"`
+ ` v-for="item of items"`

`v-for` 块中，可访问所有父作用域state。`v-for` 支持可选的第二个参数-当前项的索引。

```vue
<template>
	<div>
    <ul id="example-1">
      <li v-for="item in items" :key="item.message">
        {{ item.message }}
      </li>
    </ul>
    <!--Foo-->
    <ul id="example-2">
      <li v-for="(item, index) in items">
        {{ parentMessage }} - {{ index }} - {{ item.message }}
      </li>
    </ul>
    <!--Parent-0-Foo-->
  </div>

</template>
<script>
  var example1 = new Vue({
  el: '#example-1',
  data: {
      items: [
        { message: 'Foo' },
        { message: 'Bar' }
      ]
    }
  })
  var example2 = new Vue({
  el: '#example-2',
  data: {
    parentMessage: 'Parent',
    items: [
        { message: 'Foo' },
        { message: 'Bar' }
      ]
    }
  })
</script>

```

#### v-for遍历对象vue

```vue
<template>
  <ul id="v-for-object" class="demo">
    <li v-for="value in object">
      {{ value }}
    </li>
    <!--How to do lists in Vue-->
		<!--Jane Doe-->
    <div v-for="(value, name, index) in object">
  		 {{ index }} . {{ name }}: {{ value }}
		</div>
    <!--0.title:How to do lists in Vue-->
		<!--1.author:Jane Doe-->
  </ul>

</template>

<script>
  new Vue({
  el: '#v-for-object',
  data: {
    object: {
      title: 'How to do lists in Vue',
      author: 'Jane Doe',
    }
   }
	})
</script>
```

#### key

VDOM根据key对比渲染

```vue
<div v-for="item in items" v-bind:key="item.id">
  <!-- 内容 -->
</div>
```

### 数组更新

由于 JavaScript 的限制，Vue **不能检测**数组和对象的变化。

但**Vue将被侦听的数组的变更方法进行了包裹**，调用方法时也会**触发视图更新**。包括

+ `push()`
+ `pop()`
+ `shift()`
+ `unshift()`
+ `splice()`
+ `sort()`
+ `reverse()`

```js
example1.items = example1.items.filter(function (item) {
  return item.message.match(/Foo/)
})
```

这些包裹方法不会变更元数组而总是**返回一个新数组**，但是这不会导致Vue丢弃现有DOM重新渲染，Vue为了使得DOM元素得到最大范围的重用而实现了一些智能的启发式方法。

### v-for与v-if一起使用

我们**不**推荐在同一元素上使用 `v-if` 和 `v-for`。

当它们处于同一节点，`v-for` 的优先级比 `v-if` 更高，这意味着 `v-if` 将分别重复运行于每个 `v-for` 循环中。

```vue
//将只渲染未完成的 todo
//先走循环然后if判断
<li v-for="todo in todos" v-if="!todo.isComplete">
  {{ todo }}
</li>

//如果想跳过循环
<ul v-if="todos.length">
  <li v-for="todo in todos">
    {{ todo }}
  </li>
</ul>
<p v-else>No todos left!</p>
```

## 事件处理

### 监听事件v-on

对于复杂逻辑处理 `v-on` 可以接收一个需要调用的方法名

```vue
<div id="example-1">
  <button v-on:click="counter += 1">Add 1</button>
  <p>The button above has been clicked {{ counter }} times.</p>
</div>

<div id="example-2">
  <button v-on:click="greet">Greet</button>
</div>

<div id="example-3">
  <button v-on:click="say('hi')">Say hi</button>
</div>

<script>
   methods: {
    greet: function (event) { alert('Hello ' + this.name + '!')  },
    say: function (message) {  alert(message) }
   }
</script>
```

### 事件修饰符

在事件处理程序中调用 `event.preventDefault()` 或 `event.stopPropagation()`很常见。

尽管我们可以在方法中轻松实现这点，但更好的方式是：方法只有纯粹的数据逻辑，而不是去处理 DOM 事件细节。

所以Vue.js 为 `v-on` 提供了**事件修饰符**。之前提过，修饰符是由点开头的指令后缀来表示的。

- `.stop`
- `.prevent`
- `.capture`
- `.self`
- `.once`
- `.passive`

```vue
<!-- 阻止单击事件继续传播 -->
<a v-on:click.stop="doThis"></a>

<!-- 提交事件不再重载页面 -->
<form v-on:submit.prevent="onSubmit"></form>

<!-- 修饰符可以串联 -->
<a v-on:click.stop.prevent="doThat"></a>

<!-- 只有修饰符 -->
<form v-on:submit.prevent></form>

<!-- 添加事件监听器时使用事件捕获模式 -->
<!-- 即内部元素触发的事件先在此处理，然后才交由内部元素进行处理 -->
<div v-on:click.capture="doThis">...</div>

<!-- 只当在 event.target 是当前元素自身时触发处理函数 -->
<!-- 即事件不是从内部元素触发的 -->
<div v-on:click.self="doThat">...</div>
```

### 按键修饰符

包括常见如`.enter.tab.space.right`等等

```vue
<!-- 只有在 `key` 是 `Enter` 时调用 `vm.submit()` -->
<input v-on:keyup.enter="submit">
<input v-on:keyup.page-down="onPageDown">
```

## v-model表单输入绑定

### 基础用法

`v-model`指令可以在表单 `<input>`、`<textarea>`及`<select>`元素上创建双向数据绑定。

> `v-model` 会忽略所有表单元素的`value`、`checked`、`selected`初始值而总是将Vue 例的数据作为数据来源.
>
> 它会根据控件类型自动选取正确的方法来更新元素。尽管有些神奇，但 `v-model` 本质上不过是语法糖。它负责监听用户的输入事件以更新数据，并对一些极端场景进行一些特殊处理。
>
> ⚠️在文本区域插值 (`<textarea>{{text}}</textarea>`) 并不会生效，应用 `v-model` 来代替。

`v-model` 在内部为不同的输入元素使用不同的 property 并抛出不同的事件：

- text 和 textarea 元素使用 `value` property 和 `input` 事件；
- checkbox 和 radio 使用 `checked` property 和 `change` 事件；
- select 字段将 `value` 作为 prop 并将 `change` 作为事件。

### 修饰符

`.lazy`

在默认情况下，`v-model` 在每次 `input` 事件触发后将输入框的值与数据进行同步。你可以添加 `lazy` 修饰符，从而转为在 `change` 事件_之后_进行同步：

```vue
<!-- 在“change”时而非“input”时更新 -->
<input v-model.lazy="msg">
```

`.number`

输入值转为数值类型，可以给 `v-model` 添加 `number` 修饰符

```vue
<input v-model.number="age" type="number">
```

`.trim`

如果要自动过滤用户输入的首尾空白字符，可以给 `v-model` 添加 `trim` 修饰符

```vue
<input v-model.trim="msg">
```

## 组件

### Vue.component

组件是可复用的 Vue 实例，所以它们与 `new Vue` 接收相同的选项，例如 `data`、`computed`、`watch`、`methods` 以及生命周期钩子等。**仅有的例外是像 `el` 这样根实例特有的选项。**

#### 组件名

`Vue.component('my-component-name', { /* ... */ })`推荐全小写&连字符 or 首字母大写

#### data

组件通过data函数返回，维护一个闭包

```vue
<script>
// 定义一个名为 button-counter 的新组件
Vue.component('button-counter', {
  data: function () {
    return {
      count: 0
    }
  },
  template: '<button v-on:click="count++">You clicked me {{ count }} times.</button>'
})font

<div id="components-demo">
  <button-counter></button-counter>
</div>

new Vue({ el: '#components-demo' })
</script>
```

### Prop传递数据

⚠️即使传入的是静态数字or布尔值，仍然需要`v-bind:test='42' / 'false' / '[1,2,3]'`来告诉vue这是一个JS表达式而不是字符串

```js
Vue.component('blog-post', {
  props: ['title'],
  template: '<h3>{{ title }}</h3>'
})

<blog-post
  v-for="post in posts"
  v-bind:key="post.id"
  v-bind:title="post.title"
></blog-post>

new Vue({
  el: '#blog-post-demo',
  data: {
    posts: [
      { id: 1, title: 'My journey with Vue' },
      { id: 2, title: 'Blogging with Vue' },
      { id: 3, title: 'Why Vue is so fun' }
    ]
  }
})
```

### 监听子组件事件

父级组件可以通过 `v-on` 监听子组件实例的任意事件

```vue
<blog-post
  ...
  v-on:enlarge-text="postFontSize += 0.1"
></blog-post>
```

子组件可以通过调用内建的`$emit`并传入事件名称来触发一个事件

```vue
<button v-on:click="$emit('enlarge-text')">
  Enlarge text
</button>
```

#### 使用事件抛出一个值

`$emit`支持第二个参数

```vue
<button v-on:click="$emit('enlarge-text', 0.1)">
  Enlarge text
</button>
```

父级组件可以`$event`访问到被抛出的这个值

```vue
<template>
	<div>
    <blog-post
      ...
      v-on:enlarge-text="postFontSize += $event"
    ></blog-post>
    
    <blog-post
   	 ...
      v-on:enlarge-text="onEnlargeText"
  	></blog-post>
  </div>
</template>

<script>
  methods: {
    onEnlargeText: function (enlargeAmount) {
      this.postFontSize += enlargeAmount
    }
  }
</script>
```

#### 在组件上使用v-model

自定义事件也可以用于创建支持 `v-model` 的自定义输入组件。

```vue
<input v-model="searchText">
<!--等价于-->
<input
  v-bind:value="searchText"
  v-on:input="searchText = $event.target.value"
>
<custom-input v-model="searchText"></custom-input>
<!--例子-->
<script>
  Vue.component('custom-input', {
  props: ['value'],
  template: `
    <input
      v-bind:value="value"
      v-on:input="$emit('input', $event.target.value)"
    >
  `
})
</script>
```

### 插槽

#### 插槽内容

`<slot>`元素,类似react中的 children

如果父组件传东西了，`slot`内的默认内容会被全部覆盖

```vue
Vue.component('alert-box', {
  template: `
    <div class="demo-alert-box">
      <strong>Error!</strong>
      <slot>defaultContent</slot>
    </div>
  `
})
<alert-box url="/profile">
  <!-- 添加一个 Font Awesome 图标 -->
  <span class="fa fa-user"></span>
  Your Profile {{name}}
</alert-box>
```

#### 具名插槽

我们需要多个插槽时，可以给`<slot>`元素加一个name属性。不带name也会带有隐含的名字'default'

⚠️`v-slot`只能添加在 `<template>` 上, **除非只有默认插槽**

```vue
<!--子组件-->
<div class="container">
  <header>
    <slot name="header"></slot>
  </header>
  <main>
    <slot></slot>
  </main>
  <footer>
    <slot name="footer"></slot>
  </footer>
</div>
<!--父组件-->
<base-layout>
  <template v-slot:header>
    <h1>Here might be a page title</h1>
  </template>

  <p>A paragraph for the main content.</p>
  <p>And another one.</p>

  <template v-slot:footer>
    <p>Here's some contact info</p>
  </template>
</base-layout>
```

#### 插槽作用域

绑定在 `<slot>` 元素上的 attribute 被称为**插槽 prop**。现在在父级作用域中，我们可以使用带值的 `v-slot` 来定义我们提供的插槽 prop 的名字：

```vue
<span>
  <slot  v-slot:default="slotProps">">
    {{ slotProps.lastName }}
  </slot>
</span>
```

### 动态组件

动态切换类不需要根据state switch或者三元表达式，直接is判断

```vue
 <component v-bind:is="currentTabComponent" class="tab"></component>
```

#### keep-alive

动态切换并保持缓存

```vue
<!-- 失活的组件将会被缓存！-->
<keep-alive>
  <component v-bind:is="currentTabComponent"></component>
</keep-alive>
```

### 自定义事件

不同于组件和 prop，事件名不会被用作一个 JavaScript 变量名或 property 名，所以就没有理由使用 camelCase 或 PascalCase 了。并且 `v-on` 事件监听器在 DOM 模板中会被自动转换为全小写 (因为 HTML 是大小写不敏感的)，所以 `v-on:myEvent` 将会变成 `v-on:myevent`——导致 `myEvent` 不可能被监听到。

```js
//错误示范
this.$emit('myEvent')
//正确
this.$emit('myevent')
```

### 处理边界情况

#### 访问根实例

`this.$root`

```js
// Vue 根实例
new Vue({
  data: {
    foo: 1
  },
  computed: {
    bar: function () { /* ... */ }
  },
  methods: {
    baz: function () { /* ... */ }
  }
})

// 获取根组件的数据
this.$root.foo

// 写入根组件的数据
this.$root.foo = 2

// 访问根组件的计算属性
this.$root.bar

// 调用根组件的方法
this.$root.baz()
```

#### 访问父组件实例

`this.$parent`

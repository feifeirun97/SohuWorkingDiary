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


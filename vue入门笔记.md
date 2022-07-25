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
+ 
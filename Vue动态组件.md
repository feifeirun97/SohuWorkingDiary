### 动态组件

避免使用v-if,switch的方式来切换组件。vue中用`is`可以更简洁的实现不同组件动态切换。【注意每次切换都会创建一个新的实例】





### KeepAlive

 `<keep-alive>`将动态组件包裹起来后可以将元素状态缓存下来

```vue
<!-- 失活的组件将会被缓存！-->
<keep-alive>
  <component v-bind:is="currentTabComponent"></component>
</keep-alive>
```


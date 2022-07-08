# umi3+dva

### Intro

<img src="/Users/xufei/Library/Application Support/typora-user-images/image-20220708085212765.png" alt="image-20220708085212765" style="zoom:50%;" />

react是ui层解决方案, redux管理数据流, redux-saga处理异步数据

**dva**是基于redux和redux-saga的数据流方案, 内置了react-router和fetch

**umi**是以路由为基础, 支持配置式路由和约定式路由, 并配以完善的插件体系的企业级框架

+ 以前路由访问与服务器路径文件一一对应, router实现的是访问文件的灵活控制
+ 如果没有配置路由umi会自动进入约定式路由, 分析`src/pages`生成路由配置 

![image-20220708085622964](/Users/xufei/Library/Application Support/typora-user-images/image-20220708085622964.png)

The 

### Redux

![image-20220708104655804](/Users/xufei/Library/Application Support/typora-user-images/image-20220708104655804.png)

[Redux, Redux-thunk, Redux-saga, React-redux的区别](https://github.com/lulujianglab/blog/issues/34)

#### Redux起源 [Redux = Reducer + Flux]

![flux overview](https://hulufei.gitbooks.io/react-tutorial/content/image/flux-overview.png)

react的官方辅助数据层框架是flux, 但flux架构存在很多缺点. 如公共数据可存在多store.

flux单向数据流`Action->Dispatcher->Store->View`

**redux是在flux设计理念上引入一个新概念reducer**

#### Redux核心概念

```js
//store唯一
//只有store能改变自己的内容, reducer只是将新数据和action整理后返回给store
//reducer为纯函数 [不对传参修改]
```

#### Redux常用API

```js
//createStore
//store.dispatch
//store.getState
//store.subscribe订阅store改变, 只要store改变这个函数接受的callback自动执行
```

![数据流更新动画](http://cn.redux.js.org/assets/images/ReduxDataFlowDiagram-49fa8c3968371d9ef6f2a1486bd40a26.gif)

### Redux-thunk

如果一些复杂的逻辑和ajax请求都在组件里实现会过于臃肿[componentDIdMount和useEffect中], 可以将他们移出到其他页面统一处理, 即通过thunk把这些逻辑放到action去处理.

```js
//thunk是在store中创建的, 即redux的中间件非react的中间件
//原来action返回的是一个对象[type, newState], 使用thunk后action可以返回一个函数, 从而在函数中进行异步操作
```



















### Dva

公共数据在redux是以store的形式, 在dva中被称为model, 私有数据仍称为state

同步数据dispatch actiron -> reducer

![img](https://zos.alipayobjects.com/rmsportal/PPrerEAKbIoDZYr.png)

dva提出了订阅的概念, 一些由keyboard 输入、geolocation 变化、history 路由变化等等触发订阅数据源, 然后根据条件dispatch action。

```js
//针对常见应用场景触发数据获取 访问www.test.com/users
if (pagePath == '/users') {}
if (keyboard == 'right') {}
```




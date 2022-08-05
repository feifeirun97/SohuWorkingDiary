# umi3+dva

## Intro

<img src="/Users/xufei/Library/Application Support/typora-user-images/image-20220708085212765.png" alt="image-20220708085212765" style="zoom:50%;" />

react是ui层解决方案, redux管理数据流, redux-saga处理异步数据

**dva**是基于redux和redux-saga的数据流方案, 内置了react-router和fetch

**umi**是以路由为基础, 支持配置式路由和约定式路由, 并配以完善的插件体系的企业级框架

+ 传统路由访问与服务器路径文件一一对应, router实现的是访问文件的灵活控制。即router控制当前地址指向哪个文件
+ 如果没有配置路由umi会自动进入约定式路由, 分析`src/pages`生成路由配置 

![image-20220708085622964](/Users/xufei/Library/Application Support/typora-user-images/image-20220708085622964.png)







## Redux

![image-20220708104655804](/Users/xufei/Library/Application Support/typora-user-images/image-20220708104655804.png)

[Redux, Redux-thunk, Redux-saga, React-redux的区别](https://github.com/lulujianglab/blog/issues/34)

#### Redux起源 [Redux = Reducer + Flux]

![flux overview](https://hulufei.gitbooks.io/react-tutorial/content/image/flux-overview.png)

react的官方辅助数据层框架是flux, 但flux架构存在很多缺点. 如公共数据可存在多store.

flux单向数据流`Action->Dispatcher->Store->View`

**redux是在flux设计理念上引入一个新概念reducer**

### Redux数据流

+ `view` 触发事件`dispatch`一个 action 到`store`
+ `store`收到 action ,连同之前的 `state` 一起传给 `reducer` 
+ ` reducer` 返回新数据给 `store` ,` store` 改变
+ 组件接收到新的 `state` 重新渲染页面

![redux数据流](https://s2.loli.net/2022/08/02/nVaC6mOBI1RWgdU.gif)

### Redux核心概念

```js
//store唯一
//只有store能改变自己的内容, reducer只是将新数据和action整理后返回给store
//reducer为纯函数 [不对传参修改，对原state深拷贝]
#######补充########
//actionTypes常量拆分									原因: type拼错很难排查, 预先定义好常量
//actionCreator统一管理页面action			 原因: action不应该在组件内定义, 封装好暴露出接口即可。可维护易测试
```

### Redux常用API

```js
//createStore()
//store.dispatch(action)
//store.getState()
//store.subscribe(store.getState())订阅store改变, 只要store改变这个函数接受的callback自动执行
```





## Redux-thunk

**问题：**复杂的逻辑和ajax请求都在组件里实现会过于臃肿[componentDIdMount和useEffect中], 当项目代码量增加时, 生命周期函数夹杂着不同的异步函数和业务逻辑，会变得非常复杂难以维护。

**解决：**把这些异步请求和复杂逻辑放到其他页面统一处理, 即通过thunk把他们放到actionCreactor集中管理

+ thunk是在store中创建的, 即redux的中间件非react的中间件
+ 原来action返回的是一个对象[type, newState], 使用thunk后action可以返回一个函数, 从而在函数中进行异步操作

```jsx
//逻辑放在生命周期函数中
componentDidMount() {
  axios.get('/list.json').then((res) => {
    const data = res.data
    const action = initListAction(data)
    store.dispatch(action)
  })
}
//使用中间件引入thunk
export const store = createStore(
  reducer,
  applyMiddleware(thunk) // applyMiddleware可以使用中间件模块
) 
//放在actionCreator.js中管理
export const getTodoList = () => {
  return (dispatch) => {
    axios.get('/list.json').then((res) => {
      const data = res.data
      const action = initListAction(data)
      dispatch(action)
    })
  }
} 
//逻辑抽离后放在thunk中，action返回一个函数
componentDidMount() {
  const action = getTodoList()
  store.dispatch(action) // 调用 store.dispatch()这个函数时，action这个函数就会被执行
}
```



## Redux中间件

 实际上中间件就是对**`dispatch`的一个封装或升级。**让它可以接收对象和函数。此时如果`dispatch`接收的是对象即按之前流程走, 如果接收的是函数就等函数执行完再调`store`

![](https://s2.loli.net/2022/08/02/ImH8okiUMPp7BcK.gif)

## Redux-saga

**问题：**原先我们只能在` reducer` 里拿到 `store` 去做一些业务逻辑 [ 因为`action `只能给到` store `,` store `再把之前的数据和 `action` 给到` reducer`]。

**解决：**`redux-saga`是异步代码拆分的中间件，可以完全替代`thunk`。有了`redux-saga`之后,  `saga.js `也可以接收 action了





异步代码拆分的中间件，可以完全替代`thunk`













### Dva

公共数据在redux是以store的形式, 在dva中被称为model, 私有数据仍称为state

同步数据dispatch actiron -> reducer

![img](https://s2.loli.net/2022/08/02/rHkLdO6gtAbWJya.png)

dva提出了订阅的概念, 一些由keyboard 输入、geolocation 变化、history 路由变化等等触发订阅数据源, 然后根据条件dispatch action。

```js
//针对常见应用场景触发数据获取 访问www.test.com/users
if (pagePath == '/users') {}
if (keyboard == 'right') {}
```





## Umi基础

### 目录结构

#### 根目录

+ package.json 

  包含插件和插件集，以 `@umijs/preset-`、`@umijs/plugin-`、`umi-preset-` 和 `umi-plugin-` 开头的依赖会被自动注册为插件或插件集

+ .umirc.ts

  包含 umi 内置功能和插件的配置文件

+ .env

  环境变量如`PORT=8888;COMPRESS=none`

+ dist文件夹

  `umi build`打包后的文件

+ mock文件夹

  目录下所有ts,js都会被解析为mock文件

+ public文件夹

  此目录下所有文件会被 copy 到输出路径

+ src文件夹

  资源文件夹

#### Src文件夹

+ .umi目录

  

```js
.
├── package.json
├── .umirc.ts
├── .env
├── dist
├── mock
├── public
└── src
    ├── .umi
    ├── layouts/index.tsx
    ├── pages
        ├── index.less
        └── index.tsx
    └── app.ts
```


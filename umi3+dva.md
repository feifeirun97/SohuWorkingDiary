# umi3+dva

### Intro

<img src="/Users/xufei/Library/Application Support/typora-user-images/image-20220708085212765.png" alt="image-20220708085212765" style="zoom:50%;" />

react是ui层解决方案, redux管理数据流, redux-saga处理异步数据

**dva**是基于redux和redux-saga的数据流方案, 内置了react-router和fetch

**umi**是以路由为基础, 支持配置式路由和约定式路由, 并配以完善的插件体系的企业级框架

+ 以前路由访问与服务器路径文件一一对应, router实现的是访问文件的灵活控制
+ 如果没有配置路由umi会自动进入约定式路由, 分析`src/pages`生成路由配置 

![image-20220708085622964](/Users/xufei/Library/Application Support/typora-user-images/image-20220708085622964.png)



### Redux

[Redux, Redux-thunk, Redux-saga, React-redux的区别](https://github.com/lulujianglab/blog/issues/34)

![image](https://user-images.githubusercontent.com/26807227/48754075-6150c680-eccb-11e8-83ba-f3463b08e952.png)

![数据流更新动画](http://cn.redux.js.org/assets/images/ReduxDataFlowDiagram-49fa8c3968371d9ef6f2a1486bd40a26.gif)

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




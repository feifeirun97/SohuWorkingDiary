# React架构&源码

从理念到架构，从架构到实现，从实现到具体代码。本书划分为`理念篇`、`架构篇`、`实现篇`。

## 理念篇

> 我们认为，React 是用 JavaScript 构建**快速响应**的大型 Web 应用程序的首选方式。它在 Facebook 和 Instagram 上表现优秀。 --React官网

### **快速响应的制约场景**

+ **CPU瓶颈: **大量计算操作性能超标->卡顿掉帧
+ **IO瓶颈: **异步请求等待返回->不能快速响应

`React`是如何解决这两个瓶颈的呢？

### CPU瓶颈

> 当项目变得庞大组件繁多时, 自然而然遇到CPU（Central Processing Unit）瓶颈。

```jsx
//在视图中渲染3000个子节点
const App = () {
  const len = 3000;
  return (
    <ul>
      {Array(len).fill(0).map( (_,i)=><li>{i}</li> )}
    </ul>
  )
}
const rootEl = document.querySelector('#root')
ReactDOM.render(<App/>, rootEl)
```

#### 原因

主流浏览器刷新频率60HZ，即(1000ms/60Hz) 16.6ms浏览器刷新一次。

我们知道，JS可以操作DOM，`GUI渲染线程`和 `JS线程`是互斥的. 所以`JS执行脚本`和`浏览器布局、绘制`不能同时进行。在每16.6ms时间内，需要完成`JS脚本执行 -----  样式布局 ----- 样式绘制`。

当JS执行时间过长，超出了16.6ms，这次刷新就没有时间执行**样式布局**和**样式绘制**了。

在上述Demo中，由于组件数量大，JS执行时间过长，导致页面卡顿掉帧。如下执行堆栈图: JS执行时间73.65ms, 远远多于1帧的时间。

![long-task.png](https://s2.loli.net/2022/08/30/GncDVe2PQiuk6yM.png)

#### 解决

`React`在浏览器每一帧的时间中，预留一些时间给JS线程，利用这部分时间更新组件（在[源码](https://github.com/facebook/react/blob/1fb18e22ae66fdb1dc127347e169e73948778e5a/packages/scheduler/src/forks/SchedulerHostConfig.default.js#L119)中，预留的初始时间是5ms）。

当预留的的时间不够用时，`React`将线程控制权还给浏览器使其有时间渲染UI，`React`则等待下一帧时间来继续被中断的JS工作。

> 这种将长任务分拆到每一帧中，像蚂蚁搬家一样一次执行一小段任务的操作，被称为`时间切片`（time slice）

接下来我们开启`Concurrent Mode`（后续章节会讲到，当前你只需了解开启后会启用`时间切片`）：

```jsx
// 通过使用ReactDOM.unstable_createRoot开启Concurrent Mode
// ReactDOM.render(<App/>, rootEl);  
ReactDOM.unstable_createRoot(rootEl).render(<App/>);
```

此时我们的长任务被拆分到每一帧不同的`task`中，如下图`JS脚本`执行时间大体在`5ms`左右，这样浏览器就有剩余时间执行**样式布局&样式绘制**，降低掉帧的可能性

![长任务](https://react.iamkasong.com/img/time-slice.png)

#### 小结

所以，解决`CPU瓶颈`的关键是实现`时间切片`，而`时间切片`的关键是：将**同步的更新**变为**可中断的异步更新**。

> [拓展demo](https://codesandbox.io/s/concurrent-3h48s?file=/src/index.js)
>
> 有个更新很耗时的大列表，直观看到**同步更新**和**异步更新**时，输入框的响应速度。当牺牲了列表的更新速度，`React`大幅提高了输入响应速度，使交互更自然。



### IO瓶颈

`网络延迟`是前端开发者无法解决的。如何在`网络延迟`客观存在的情况下，减少用户对`网络延迟`的感知？

`React`给出的答案是[将人机交互研究的成果整合到真实 UI 中](https://zh-hans.reactjs.org/docs/concurrent-mode-intro.html#putting-research-into-production)。

这里我们以业界人机交互最顶尖的苹果举例，在IOS系统中：

点击“设置”面板中的“通用”，进入“通用”界面

### Hook

#### useMemo

[stackoverflow答案](https://stackoverflow.com/questions/66429202/what-are-production-use-cases-for-the-useref-usememo-usecallback-hooks)



#### useRef

```js
function App() {
  const ordinaryObject = { current: 0 } // It will reset to {current:0} at each render
  const refObject = useRef(0) // It will persist (won't reset to the initial value) for the component lifetime
  return <>...</>
}
```

useRef返回一个**可变对象**, 它的值会在组件**生命周期持续存在**而不用担心重新渲染带来的初始化。

ref 对象内容发生变化时，`useRef` 并不会通知。变更 `.current` 属性不会引发组件重新渲染。如果想要在 React 绑定或解绑 DOM 节点的 ref 时运行某些代码，则需要使用[回调 ref](https://zh-hans.reactjs.org/docs/hooks-faq.html#how-can-i-measure-a-dom-node) 来实现。



//操作原生dom

//父组件调用子组件


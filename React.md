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





### 受控组件

> [文章链接](https://juejin.cn/post/6858276396968951822)
>
> 后续文章未读[在实际业务中如何灵活运用受控组件与非受控组件](https://zhuanlan.zhihu.com/p/37579677)

#### 引子

对某个组件状态对掌控，他的值是否只能由用户设置，而不是代码控制。

假设选择用react定一个输入框。当用户输入内容时它自己维护了一个state(这里指input元素自带的value)。

如果我们想去控制输入框的内容，则我们需要定义react的state以及onchange拿到输入框的value值。

这样不论用户输入什么内容state和ui都会跟着更新，并且我们可以调用和修改input里的内容。

```jsx
class TestComponent extends React.Component {
  constructor (props) {
    super(props);
    this.state = {
      username: "lindaidai"
    }
  }
  onChange (e) {
    console.log(e.target.value);
    this.setState({
      username: e.target.value
    })
  }
  render () {
    return <input name="username" value={this.state.username} onChange={(e) => this.onChange(e)} />
  }
}
```

#### 定义

在HTML的表单元素中，它们通常自己维护一套`state`，并随着用户的输入自己进行`UI`上的更新，这种行为是不被我们程序所管控的。而如果将`React`里的`state`属性和表单元素的值建立依赖关系，再通过`onChange`事件与`setState()`结合更新`state`属性，就能达到控制用户输入过程中表单发生的操作。被`React`以这种方式控制取值的表单输入元素就叫做**受控组件**。



### 非受控组件

#### 引子

当我们仅想要获取某个表单元素的值，而不是关心它如何改变。可以通过获取DOM元素信息的方式来获取表单的值即`ref`

在输入框输入内容后，点击提交按钮，我们可以通过`this.inputRef`成功拿到`input`的`DOM`属性信息，包括用户输入的值，这样我们就不需要像受控组件一样，单独的为每个表单元素维护一个状态。同时我们也可以用`defaultValue`属性来指定表单元素的默认值。

⚠️`file`表单控件始终是一个不受控制的组件，因为它的值只能由用户设置，而不是以编程方式设置。如果通过setstate拿值会报错需要用非受控组件来获取它的值。

```jsx
import React, { Component } from 'react';
export class UnControll extends Component {
  constructor (props) {
    super(props);
    this.inputRef = React.createRef();
  }
  handleSubmit = (e) => {
    console.log('我们可以获得input内的值为', this.inputRef.current.value);
    e.preventDefault();
  }
  render () {
    return (
      <form onSubmit={e => this.handleSubmit(e)}>
        <input defaultValue="lindaidai" ref={this.inputRef} />
        <input type="submit" value="提交" />
      </form>
    )
  }
}
```

#### 总结

用React官方的话来说，绝大部分时候推荐使用`受控组件`来实现表单，因为在受控组件中，表单数据由`React`组件负责处理；当然如果选择`受受控组件`的话，表单数据就由`DOM`本身处理。


























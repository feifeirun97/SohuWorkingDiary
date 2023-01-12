## 异同点

### 相同点

+ state贡献数据
+ 流程一致：定义全局state,触发,修改state
+ 原理相似，都是从flux衍生出。通过全局注入store

### 表现层不同点

+ vuex定义了`state,getter,mutation,action `四个对象。redux定义了`state,reducer,action`

+ vuex中state统一存放，方便理解。redux中state依赖于所有reducer的初始值。

+ vuex有getter快速得到state。redux没有这层。`react-redux mapStateToProps`参数做了这个工作。

+ vuex中

  

  




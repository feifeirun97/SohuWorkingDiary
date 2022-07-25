## Week1-2

> uploadRecords的post请求起到什么作用？

```js
//一些监控上传
//行为监控，做一些统计
//dofin-sever就是报错统计
```

![image-20220711103117880](/Users/xufei/Library/Application Support/typora-user-images/image-20220711103117880.png)

> 什么时候写type？

```js
//在pages各文件夹目录下定义接口入参及返回Promise的类型格式
//通过Declare的方式全局都可以调用类型约束Services.
//图中接口.then接收参数没有去类型约束是因为在别的地方已经定义了
```

![image-20220711103842638](/Users/xufei/Library/Application Support/typora-user-images/image-20220711103842638.png)

> 接口走通

![image-20220711110352514](/Users/xufei/Library/Application Support/typora-user-images/image-20220711110352514.png)

> 受控与非受控组件的区别？

```js
//在HTML的表单元素中，它们通常自己维护一套`state`，并随着用户的输入自己进行`UI`上的更新，这种行为是不被我们程序所管控的。即非受控组件

//而如果将`React`里的`state`属性和表单元素的值建立依赖关系，再通过`onChange`事件与`setState()`结合更新`state`属性，就能达到控制用户输入过程中表单发生的操作。被`React`以这种方式控制取值的表单输入元素就叫做受控组件。
```

> flex自适应。flex 11代表什么？gap如何操作？**

```js
//代表所占位置的大小
```

> 多个文字+筛选框占比四等份怎么做？同时有间隔20要求

```jsx
//method1: flex gap or grid
//method2: antd Row[gutter20] + Col[span=24/4=6]
//如果单独设置margin-right,还需要为类选择器消除最后一个margin, 同时不够灵活适应多行的情况
<div className={styles.rowContainer}>
  <Row gutter={20}>
    <Col span={6} className={styles.contentFiltersItem}>
      <span className={styles.contentFiltersItemLabel}>选择时间</span>
      <DatePicker.RangePicker />
    </Col>
  </Row>
</div>
//css
 .content {
    padding: 0 24px 24px 24px;
    .rowContainer {
      position: relative;
      width: 100%;
      height: 100%;
      margin-bottom: 24px;
      overflow: hidden;
      .contentFiltersItem {
        display: flex;
        align-items: center;
        padding-top: 24px;
        .contentFiltersItemLabel {
          margin-right: 10px;
          color: rgba(0, 0, 0, 0.85);
          font-size: 14px;
          line-height: 24px;
        }
      }
    }
 }

```

> pages下组建目录规范？

```js
//一级目录
>index.tsx主文件
>index.d.ts类型约束
>style.less公共样式
>utils公共函数
//组件二级目录
>index.tsx
>index.less
```

<img src="/Users/xufei/Library/Application Support/typora-user-images/image-20220713095521953.png" alt="image-20220713095521953" style="zoom: 67%;" />

> routes配置不生效

```js
//微前端架构 - 父项目有自己的routes，父项目在接口网站配置的不走routes.ts

//子项目配置的路由在微前段基座里匹配当前浏览器中的路由。需要在父项目配置
```



## Week3

> 接口走通decorator.ts

```tsx
Cons中包含的是什么？
//常量汇总，先不用管
注释风格@？
//前段约定俗成的规范 https://www.html.cn/doc/jsdoc/howto-es2015-modules.html
Promise<T>怎么理解？ 
//promise原生自带api适配ts
//Promise<T>的泛型T代表成功态后resolve的值，resolve(value)
request extend？
//https://github.com/umijs/umi-request/blob/master/README_zh-CN.md#创建实例
//https://blog.csdn.net/dazeka/article/details/123231400
removeAllunfefKey递归删除空值？
//不用管仅删空值, 留给后端也行
request.ts中的Response类型？
//umi自带
```

> 接口报错？

```js
//copy cUrl私聊后端
```

![image-20220719150917918](/Users/xufei/Library/Application Support/typora-user-images/image-20220719150917918.png)

> useRef起到什么作用？

```js
//useRef返回一个**可变对象**, 它的值会在组件**生命周期持续存在**而不用担心重新渲染带来的初始化。
//开发中常用来获取dom节点或者子组件的函数/state。一些封装组件也会暴露一些ref接口
```

> useEffect不能asnyc Effect调用接口如何解决？

```js
//第三方库react-use useAsync
//立即执行函数
//在里面定义一个async函数再去调用
```

> 正向代理和反向代理区别？

```js
//正向代理用户知道目标地址，类似vpn
//反向代理用户不知道目标地址，只访问中间服务器
```

> vim常规指令

```js
//i进入编辑
//esc退编辑
//:wq保存退出
```

> h5开发host配置流程？

```js
//1.修改hosts文件达到映射效果
//2.访问qw-h5.focus-test.cn实际上映射访问了127.0.0.1:80
//3.通过ngnix反向代理监听80端口 =>转到 88接口【即项目地址】
//4.请求全走ngnix

```

> 极客云开发host配置流程？

```js
//1.修改hosts文件达到映射效果
//2.访问focus-test2.cn实际上映射访问了127.0.0.1:80
//3.为了解决跨域问题。我们后台接口白名单有focus-test2.cn

```



## Week4

> 最大字端长度？

```css
/*不要用js控制,要每个地方都去也一个三元表达式.用css判断*/
		overflow: hidden;
    white-space: nowrap;
    text-overflow: ellipsis;
    word-break: break-all;
text-overflow:    用来设置是否使用一个省略标记（···）标示对象内文本的溢出。
white-space:nowrap;     强制文本在一行内显示。
overflow：hidden;    溢出内容为隐藏。
```

> 开发不顺畅各种问题？

```js

产品文档存在很多不明确的地方

+ 群发内容文件，小程序，视频，点击打开方式是跳转或者放大没写
+ 内容条数多了是翻页还是滚动下滑没说
+ 刷新功能不明确。
+ 文件消息类型描述不明确。文件大小展示后段接口并没有，设计稿上有，点击查看方式没写。

UI稿很多内容靠猜，前端实现后发现不是想要的效果

后端接口很多和UI稿不符合

沟通非常浪费时间
```

> 默认展示图片

```js
//imageFallback
//default-avator

```



### todo

#### file：

后端标题字段没有。

Ui两行隐藏，

跨域下载：网络资源转换成文件流才能下载。组内还未做过。File



#### 转发结果列表：

后段头像字段没有



#### ❓Tooltip

ui

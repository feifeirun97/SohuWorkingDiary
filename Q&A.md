## Week1-2

> Redux中的中间件redux-thunk处理异步 和 redux-saga有什么区别？

```js
//网上查找后大一些理解：
//saga是react中间件,旨在更好地解决异步操作(effect action),相当于在Redux原有数据流中多了一层，对action监听.
//
```

> uploadRecords每分都在post请求

```js
//一些监控上传
```

![image-20220711103117880](/Users/xufei/Library/Application Support/typora-user-images/image-20220711103117880.png)

> 什么时候写type？

![image-20220711103842638](/Users/xufei/Library/Application Support/typora-user-images/image-20220711103842638.png)

> 接口走通

![image-20220711110352514](/Users/xufei/Library/Application Support/typora-user-images/image-20220711110352514.png)

> 受控与非受控组建的区别？

```js
//
```

> flex自适应。flex 11代表什么？gap如何操作？

```js
//
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
>utils接口调用
//组件二级目录

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
removeAllunfefKey递归删除空值？
//不用管仅删空值, 留给后端也行
request.ts中的Response类型？
//

```





### 前端技术分享

> 为什么所有请求都要放在services？而不是分散在各个组件中？

```

```


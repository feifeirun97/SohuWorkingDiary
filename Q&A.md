object-fit：cover



> [探马账号](https://account.tanmarket.cn/#/login?corpId=ww2c6fbc86550ffe2f)

```js
//https://account.tanmarket.cn/#/login?corpId=ww2c6fbc86550ffe2f
//admin_cjxh0426
//Cj112358
```

## Week1-2

> uploadRecords的post请求起到什么作用？FocusSohu78!

```js
//一些监控上传
//行为监控，做一些统计
//dofin-sever就是报错统计
```

![image-20220711103117880](/Users/xufei/Library/Application Support/typora-user-images/image-20220711103117880.png)

> 受控与非受控组件的区别？

```js
//在HTML的表单元素中，它们通常自己维护一套`state`，并随着用户的输入自己进行`UI`上的更新，这种行为是不被我们程序所管控的。即非受控组件

//而如果将`React`里的`state`属性和表单元素的值建立依赖关系，再通过`onChange`事件与`setState()`结合更新`state`属性，就能达到控制用户输入过程中表单发生的操作。被`React`以这种方式控制取值的表单输入元素就叫做受控组件。
```

> 什么时候写type？

```js
//在pages各文件夹目录下定义接口入参及返回Promise的类型格式
//通过Declare的方式全局都可以调用类型约束Services.
//图中接口.then接收参数没有去类型约束是因为在别的地方已经定义了
```

![image-20220711103842638](/Users/xufei/Library/Application Support/typora-user-images/image-20220711103842638.png)

> 接口走通

![image-20220711110352514](/Users/xufei/Library/Application Support/typora-user-images/image-20220711110352514.png)

> flex自适应。flex 11代表什么？gap如何操作？**

```jsx
//代表所占位置的大小
<div class="flex-gap">
  <div>1</div>
  <div>2</div>
  <div>3</div>
</div>
.flex-gap {
  display: inline-flex;
  flex-wrap: wrap;
  gap: 12px;
}

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

//子项目配置的路由需要在父项目案场基座配置, 极客云项目通过请求返回的JSON字段来渲染菜单栏和用户权限
```



## Week3

> h5开发host配置流程？

```js
//1⃣️修改hosts文件达到映射效果
//2⃣️访问qw-h5.focus-test.cn实际上映射访问了127.0.0.1:80
//3⃣️通过ngnix反向代理监听80端口 =>转到 88接口【即项目地址】
//4⃣️请求全走ngnix

```

> 接口走通decorator.ts问题合集？

```tsx
Cons中包含的是什么？
//常量汇总
注释风格@？
//前端约定俗成的规范 https://www.html.cn/doc/jsdoc/howto-es2015-modules.html
Promise<T>怎么理解？ 
//promise原生自带api适配ts
//Promise<T>的泛型T代表成功态后resolve的值，resolve(value)
request extend？
//https://github.com/umijs/umi-request/blob/master/README_zh-CN.md#创建实例
//https://blog.csdn.net/dazeka/article/details/123231400
removeAllunfefKey递归删除空值起什么作用？
//删空值函数
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

> 默认展示图片？

```js
//imageFallback
//default-avator
```

> 给一个url如何实现点击下载？

```js
//方法一失败
<a href="http://www.test.com/down/name.excel" download="name">
//方法二失败
const downloadURI = (uri, name) => {
  const link = document.createElement("a");
  link.download = name;
  link.href = uri;
  document.body.appendChild(link);
  link.click();
  document.body.removeChild(link);
}
//方法三转文件流失败
blob()

//原因：腾讯云跨域 + 部分资源有问题

//最终原因是图片资源有问题
//后端上传失败了资源错误了，等后端解决
```

> 配置基坐路由？

```js
//https://configuration-platform.focus.cn/#/detail/62afdc13bb7ac8858b2fcdb5
//smartCase-test.json路由文件通过接口发过来
//项目列表 - 极客云路由详情 - 测试环境 - 编辑 - 修改 - 备注 - 生成新文件
//正式环境不要动
```

> 中文占2字符，总长不超过40如何计算？

```js
//utils.string -getLength方法
```

## Week5

> Pro自封组件流程-基于dumi, 如何操作？

```js
//pro-components本地 
//新建分支到pakage文件夹下
//focus add pro-header
//选择package
```

> 为何modal内tabs全局覆盖样式未生效？

![在这里插入图片描述](https://s2.loli.net/2022/08/02/CU4RS8ANHFXLVYI.png)

```js
//❓原因:  弹框节点和项目节点同级，被挂载到外面了，覆盖样式不生效
//✅方案1: 给Modal加属性 getContainer = {false} 让它挂载进父节点。
//✅方案2: 直接封装成pro组件调用。	
```

> 为何到三级路由用push(), 返回用replace？

```js
//window.history.xxx()
push -- 向history栈里面添加一条新记录，这个时候用户点击浏览器的回退按钮可以回到之前的路径；
go -- 这个方法的参数是一个整数，意思是在 history 记录中向前或者后退多少步，类似 window.history.go(n)，n可为正数可为负数；
goBack -- 返回上一页，相当于window.history.go(-1);
replace -- 替换掉当前的 history 记录，跳转到指定的url，这个方法不会向history里面添加新的记录，点击返回，会跳转到上上一个页面。上一个记录是不存在的；

//在history中没有的记录，用push跳转，如果有即将跳转的页面这条记录了，就需要用go来返回到指定页面，不能再push，否则可能会导致返回事件不能按照我们预期的进行。如果是要替换当前页面，那就用replace。
//【注意】Umi3封装了router要依照文档
```

> antd table里的inline-block自带内边距如何消除？ 
>

```js
//给父级加一个font-size: 0
```

> UI稿二级页面内边距如何定位的?

```js
//不是定死的内边距
//如果内容第一个是标题上padding20，如果不是上padding20
```

> Antd栅格系统?

```js
//一行24个单位，通过span控制占位，通过gutter控制间隔
//通过offset控制偏移。offset=4向右移4个单位。栅格左侧的间隔格数，间隔内不可以有栅格
```

> 为什么项目中没有Umi3官方文档中文件夹

```js
//smart-case
//缺少.umirc.ts - .env - .dist
//smart-qw
//缺少.umirc.ts - .dist
//原因: 都写在config里面了
```

> 为什么React官方文档不推荐过多使用Ref？

```jsx
//因为 React 推崇 数据不可变（immutable）的编程思想，即不会直接修改对象的属性，而是深拷贝一份新对象，再对新的对象进行修改。
//在 React 中每次 setState 传递的都是一个新的对象（新的引用），在更新渲染时 React 获取的也是新的对象，而 refs 在组件的每一个生命周期都保持唯一引用，修改 ref.current 的值使得其违背了 immutable 原则。
```

## Week6

> Form表单深层嵌套多个Form表单, 如何传值&校验?

```js
//🤔设想1:全部通过Ref来操作子表单的校验,不需要自己写校验。
//❌深层嵌套至多20个表单全通过Ref操作会大大增加代码复杂性和可维护性, 虽然省去了写校验的时间但是不方便后续的组件封装使用。

//🤔设想2:全部封装为受控组件,需要手动写校验规则。 
//✅所有的子表单组件都封装成受控组件，在最上层表单校验。即方便封装代码，也增加了可读性和复用性。
```

> 拖放组件库解决方案？

```js
//🤔尝试1:看到项目中已有方案拖动解决方案是RND库
//❌与组长沟通后发现并不适合本次场景, 此处拖放组件是表单组件, 需要实时校验。

//🤔尝试2:Antd表格组件推荐了react-sortable-hoc库
//❌react-sortable-hoc填写link组件会强制重渲染并造成失焦, 只适合简单的静态拖拽方案

//🤔尝试3:多次搜索尝试后选择了Sortable.js
//✅比较好的解决了问题
```

> 为什么pro-components的打包没有出es和lib？

```js
//与克庭组长沟通后:
//在fatherrc里面加一下配置才行
```

> 如何整个文件ts忽略报错

```js
//单行忽略
// @ts-ignore

//忽略全文
// @ts-nocheck

//取消忽略全文
// @ts-check
```

> 判断对象中部分元素不用去map或者forEach

```jsx
//用array.some()
 const skipLogin = skipLoginList.some((url) =>
      window.location.href?.match(url)
 )
 if (skipLogin) return // 在skipLoginList中的路由不需要登录
 tryLogin()
```



## Week7

> 升级pro组件

```js
//focus check -f
```

> yarn add 报错
>

```js
//原因是安装的库不在focus源，切换为淘宝源即可。但是注意rscode会报错找不到该源
// yrm ls
// yrm use -x
```

> 三元表达式优化=>强制类型转换？

```js
//!!两次转换
```

> common-form样式不需要影响到嵌套的form？⚠️未解决

```js
//UI整改统一解决
```

## Week8

> ref如何暴露/接取？

```js
//子组件暴露给父组件需要useImperativeHandle
//useImperativeHandle 可以让你在使用 ref 时自定义暴露给父组件的实例值。在大多数情况下，应当避免使用 ref 这样的命令式代码。useImperativeHandle 应当与 forwardRef 一起使用：
```

> Tab组件中的内容通过DIsplay:none样式控制DOM不生效？

```js
//Tab组件自带一个缓存，通过样式无法控制
//可以搭配时间戳和key来强制刷新，注意在tab内限制接口请求
```

## Week9

> 为什么不推荐用index作为react的key呢？

```js
//当使用数组的索引作为键时，React 将优化而不是按预期呈现。在这种情况下会发生什么可以用一个例子来解释。

//假设父组件获取一个包含 10 个项目的数组，并根据该数组渲染 10 个组件。假设从数组中删除第 5 项。在下一次渲染时，父级将收到一个包含 9 个项目的数组，因此 React 将渲染 9 个组件。这将显示为第 10 个组件被删除，而不是第 5 个，因为 React 无法根据索引区分项目。

//因此，始终使用唯一标识符作为从项目数组呈现的组件的键。
```

> antd表单组件校验抖动？

```jsx
//自带的无法控制
//源码中用了两个动画延迟控制
```

> 一个表达式判断undefined 和 null？

```jsx
//if (!x)
```

## week10&week11

> 多分支合并代码&冲突？

![image-20220914094318875](/Users/xufei/Library/Application Support/typora-user-images/image-20220914094318875.png)

```js
//branch0913 和branch0901确保都和主分支dev2,3保持最新
//直接吧0913合到0901上
//合并冲突代码，左侧为others，右侧为yours。最下方为最终合并代码
//弹窗点击
//1.continue with confilicts 

```

## Week12

> H5配置开发

```js
//App.vue把tryLogin()注释掉->可以在浏览器调试
//store-modules-useData.ts->手动设置token跳过企微的登陆
//访问http://127.0.0.1:8088/pages/datas/integration/index调试
getters: {
  token: (state: StateType) => {
    return (
      state.token ||
      Cookies.get('token') ||
      'MXw5fDZ8MTcwfDE2NjM5MDI0ODEwOTl8azJLR21zTnRUMWZIZnNWZzFkZ25Kd0NONmhUSVF0WEZaZTRGcUdNdUV5ZmdxU29qK1dzOEpKTGRtNGVTYnZMNW96d0dteExPRmxDZXBPSUNpa3NaQlZjMWF3YVV0QXh1dWtSOTJtSzhUVVR1SUJYZUd2YlpYc1dWN0dxYWlmd2dQVGM3WWMzMCswMVlNc3VQM05zY1FER3BlbHVGd1pyT3laOGFLRHlsdVJvPQ'
    )
  }
}
//切换test
http://localhost:8088/pages/market/marketList/index?apiEnv=test2
//使用新组件
//useVant.ts=> Vues.use{xxx}

```

> H5代码规范

```js
//新建页面需要pages.json配置 & pages/新建文件夹
//公共组件放在components文件进行，以u-xxx命名，自动import

```

> Grid布局实现？

![image-20220922112600535](/Users/xufei/Library/Application Support/typora-user-images/image-20220922112600535.png)

```js
//对想要占多行的块元素增加
//grid-column-start: 2;
//grid-column-end: 4;
```

> 如何实现flex不溢出？

```js
//flex:1 overflow:hidden
```

## Week13

> nginx路径，配置

```js
//获取token
//关掉hosts和nginx - 企微R-token
//配置token
//constants
//hosts
//vim /etc/hosts
//http://127.0.0.1:8088/pages/datas/integration/index

```

> pro-components发版

```js
//首先在changelog.md新增feature
//🎸  @focus/pro-upload@1.1.1: maxSize 配置为 0 时，不限制图片体积
//其次找到改的组件 package.json 版本号次版本+1
//npm run publish
```

> 移动端1px问题-下划线1px在移动端展示出现粗细不一致问题，[文章](https://zhuanlan.zhihu.com/p/100752129)

```js
//CSS像素是虚拟像素,即px是一个相对单位
//设备像素是物理像素,是固定的
//主要是跟一个东西有关，DPR(devicePixelRatio) 设备像素比，它是默认缩放为100%的情况下，设备像素和CSS像素的比值。
window.devicePixelRatio=物理像素 /CSS像素
目前主流的屏幕DPR=2 （iPhone 8）,或者3 （iPhone 8 Plus）。拿2倍屏来说，设备的物理像素要实现1像素，而DPR=2，所以css 像素只能是 0.5。一般设计稿是按照750来设计的，它上面的1px是以750来参照的，而我们写css样式是以设备375为参照的，所以我们应该写的0.5px就好了啊！ 试过了就知道，iOS 8+系统支持，安卓系统不支持。

```

## Week14-15

> axios设置了withCredentials为什么不携带cookie？

```js
//跨域不会携带cookie
//request headers可以塞参数，但是不能直接修改cookie，会报错unsafe
//只能先document.cookie='key=vaule'然后withCredentials
//原因：极客云后台是因为后端配置了二级域名一致就不跨域，hosts。
```

> 如何判断是测试环境？

```js
//IsWorkEnv
```

> 集团版拷贝项目需要修改哪些东西？
>

```js
//Serverconf
//smart-qw => smart-group-qw
//smartQw => smartGroupQw
//nigix每个分支
⚠️缺少集团版权限
//集团详情-企业微信tab-新增开通单-sopid写0

//http://smart-case-admin.focus-test.cn/
//集团后台
```

## week19

> 微信开发平台登陆

```js
//https://open.weixin.qq.com/wxaopen/serviceprovider/AppList?token=bec2cef2572655239a22ce383be946ceee6d5e06

//challenking@163.com
//bodian123
```



> pfd

```
npm run dev:only
微信开发者工具
小程序 +号 路径dist/dev/mp-weixin
```

初始化配置![](/Users/xufei/Library/Application Support/typora-user-images/image-20221108182404482.png)

锁定页面开发

![image-20221108182346618](/Users/xufei/Library/Application Support/typora-user-images/image-20221108182346618.png)

> 
>
> 
>
> 
>
> 基座项目发布订阅模式，用户信息，怎么从基座传递子项目，子项目怎么接收
>
> app.tsx
>
> BUs ，emmiton



> 
>
> rscode 怎么起sever， 怎么html，复制到哪个路径
>

第一次接触到docker相关，后续业务开发之余抽时间看

截图label+宽度

> 
>
> 
>
> 

## Week20

> 图片自适应展示？

```js
object-fit: contain;
object-fit: cover
```

> 如何手动加朦层？

```css
 .imgContainer {
    position: relative;
    width: 60px;
    height: 60px;
    &:hover {
      .bg {
        z-index: 10;
      }
    }

    .picture {
      width: 100%;
      height: 100%;
      object-fit: contain;
    }
    .bg {
      position: absolute;
      top: 0;
      left: 0;
      z-index: -10;
      display: flex;
      align-items: center;
      justify-content: space-around;
      width: 100%;
      height: 100%;
      background: #00000080;
    }
  }
```

## Week24

> 下载如何拿到header中的文件名？

```
默认情况下，header只有六种 simple response headers （简单响应首部）可以暴露给外部：
Cache-Control
Content-Language
Content-Type
Expires
Last-Modified
Pragma

需要在后端设置
response.setHeader("Access-Control-Expose-Headers", "Content-Disposition")
response.setHeader("Content-Disposition", ...)
```

## Week27

> pnpm

```
版本要求 v6
pnpm install
pnpm add pkg
pnpm start
```

> 新项目启动

```
1.

```















### 申请Rscode

1. 活动中心项目版
2. 活动中心集团版







老大好，大家好：

  "活动中心接入智慧案场"需求，由于使用微前端设计智慧案场架构，子应用需要申请一个项目（目的是子应用自治）, 用来单独发布此子应用的前端资源。
  项目名称：smart-activity，类型 node，域名 smart-case.focus.cn
  中文名称：活动中心后台-前端项目
  项目说明：单独发布活动中心后台的前端资源

  多谢！

Regards~





老大好，大家好：

  "活动中心集团版接入智慧案场"需求，由于使用微前端设计智慧案场架构，子应用需要申请一个项目（目的是子应用自治）, 用来单独发布此子应用的前端资源。
  项目名称：smart-group-activity，类型 node，域名 smart-group-qw.focus-test.cn 
  中文名称：活动中心集团版后台-前端项目
  项目说明：单独发布活动中心集团版后台的前端资源

  多谢！

Regards~







2.13-2.17



专题页测试跟进&bug修复 2.13日-2.17日

活动中心产品启动 2.16日-2.17日







## 项目概述

<img src="/Users/xufei/Library/Application Support/typora-user-images/image-20230217144202258.png" alt="image-20230217144202258" style="zoom: 50%;" />

第三方产品嵌入智慧案场 [项目&集团]

我们目前的产品方案是：携带cookie给后端，后端完成接入生成并返回一个兔展的访问链接，链接中携带有用户的信息

### 方案一

直接在smartcase编写，原有嵌入逻辑改成第三方产品网站，做一些跳转、接口封装

**优点：**简单快速

**缺点：**侵入式代码

**可能遇到的问题：**二级域名不一致，cookie等

![image-20230217114306768](/Users/xufei/Library/Application Support/typora-user-images/image-20230217114306768.png)



### 方案二

新申请2个rscode项目，然后在前端页面的一个空白区域中，定义一个iframe，再对iframe设置此链接。

**优点**：业务分离

**缺点：**麻烦







1. ## 




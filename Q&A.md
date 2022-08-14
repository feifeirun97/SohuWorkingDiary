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

> 默认展示图片？

```js
//imageFallback
//default-avator
```

> 给一个url如何实现点击下载？⚠️未解决

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

//解决：腾讯云跨域

//最终原因是图片资源有问题
//后端上传失败了资源错误了，等后端解决
```

> 配置基坐路由？

```js
//smartCase-test.json路由文件通过接口发过来
//详情 - 测试环境 - 编辑 - smartQw - 修改 - 备注 - 提交
//正式环境不要动
```

> 中文占2字符，总长不超过40如何计算？

```js
//utils.string -getLength方法
```

## Week5

> Pro自封组件流程-基于dumi

```js
//pro-components本地 
//新建分支到pakage文件夹下
//focus add pro-tabs
//选择package
```

> modal内tabs全局覆盖样式未生效

![在这里插入图片描述](https://s2.loli.net/2022/08/02/CU4RS8ANHFXLVYI.png)

```js
//原因：弹框节点和项目节点同级，被挂载到外面了，覆盖样式不生效
//解决：+ 给Modal加属性 getContainer = {false} 让它挂载进来。
//		 + 直接封装成pro组件调用。	
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

> UI稿二级页面内边距如何定位的

```js
//不是定死的内边距
//如果内容第一个是标题上padding20，如果不是上padding20
```

> Antd栅格系统

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
//都写在config里面了
```

## Week6

> 为什么Form.item下面写多个组件实现传值有问题?

```js
//Form.item下面只能存在一个受控组件
//Form支持valuesOnchange
```

> 为什么pro-components的打包没有出es和lib？⚠️未解决

```js
//等待与克庭沟通
```

> 拖放是用啥做的？⚠️未解决

```js
//项目中已有方案是RND和sortablejs，但不适合本次场景
//Antd官方推荐react-sortable-hoc
//不行！待解决
```

> 如何整个文件ts忽略报错⚠️未解决

```js
//
```

> 判断对象中部分元素不用去map或者forEach

```jsx
//用array.some()
```



## Week7





请选择客户

发布+客户数0再提示



```
//inline-block自带外边距清除
.clear-inline-block-margin {
  font-size: 0;
  -webkit-text-size-adjust: none;
}

```

许飞 

1、极客云-任务中心开发(8.8 - 8.10)

2、极客云-任务中心联调(8.10 - 8.12)

#### Todo1

员工群发记录骨架品

Form ={form}子form实例绑定到父form

url正则匹配

部门type1

员工type2

#### #Todo2

Due 8 12th

inputwithnumber

AdapUser组件添加员工

AddTagsFormItem添加标签 参考filtertable，吧antd拿过来坐一个受控组件



**Todo List**

+ 字数InputWithNumber
+ 超出20个任务toast提示
+ 表单校验
  + 整体报错 help 请求取消
  + ref useForm

+ 大小定死

+ 文本输入框

  useImperativeHandle







```
import UploadFile from './UploadFile';
import UploadImg from './UploadImage';
import { Button, Image, message } from 'antd';
import React, { forwardRef, useCallback, useEffect, useRef, useState } from 'react';
import UploadVideo from './UploadVideo';
import UploadLink from './UploadLink';
import { SortableContainer, SortableElement, SortableHandle } from 'react-sortable-hoc';
import { arrayMove } from 'react-sortable-hoc';
import MenuIcon from '@/assets/menu.png';
import styles from './index.less';
import { useForm } from 'antd/lib/form/Form';

let valuesList = [];

const demo = {};
const UploadBox: React.FC<any> = (props) => {
  const [openValidate, setOpenValidate] = useState<boolean>(false);
  // const [valuesList, setValuesList] = useState<any>({});
  const [curValue, setCurValue] = useState();
  const [list, setList] = useState<any>([
    {
      id: 2,
      type: 'image',
      completed: false,
      values: [],
    },

    {
      id: 5,
      type: 'link',
      completed: false,
      values: [],
    },
  ]);
  useEffect(() => {
    setCurValue(props.value);
  }, [props.value]);
  const onSubValuesChange = (values: any, itemId: number) => {
    // setValuesList((pre) => ({ ...pre, [itemId]: values }));
    valuesList = values;
    // demo = { ...demo, [itemId]: values };
    // props.onChange(demo);
    console.log('valuesList 123', valuesList);
    console.log(values, itemId);
  };

  const onSortEnd = ({ oldIndex, newIndex }) => {
    setList(arrayMove(list, oldIndex, newIndex));
  };

  //----------------------------------------------------------------
  useEffect(() => {
    console.log(valuesList);
  }, [valuesList]);

  const renderUpload = useCallback((item: any) => {
    switch (item.type) {
      case 'file':
        return (
          <div
            key={item.id}
            style={{
              width: 700,
              borderRadius: '2px',
              border: '1px solid rgba(0,0,0,0.1500)',
              padding: '20px 12px',
            }}
          >
            <UploadFile />
          </div>
        );
      case 'link':
        return (
          <div
            key={item.id}
            style={{
              width: 700,

              borderRadius: '2px',
              border: '1px solid rgba(0,0,0,0.1500)',
              padding: '20px 12px',
            }}
          >
            <UploadLink
              // asyncValues={curValue?.[item.id]}
              // openValidate={openValidate}
              onChange={(values: any) => onSubValuesChange(values, item.id)}
            />
          </div>
        );
      case 'video':
        return (
          <div
            key={item.id}
            style={{
              width: 700,
              borderRadius: '2px',
              border: '1px solid rgba(0,0,0,0.1500)',
              padding: '20px 12px',
            }}
          >
            <UploadVideo />
          </div>
        );
      case 'image':
        return (
          <div
            key={item.id}
            style={{
              width: 700,
              borderRadius: '2px',
              border: '1px solid rgba(0,0,0,0.1500)',
              padding: '20px 12px',
            }}
          >
            <UploadImg
              onChange={(sourceUrl: string, mediaId: string) => {
                list.forEach((ele) => {
                  if (ele.id == item.id) {
                    ele.completed = true;
                    ele.mediaId = mediaId;
                    ele.url = sourceUrl;
                  }
                });
                setList([...list]);
              }}
              value={item.url}
            />
          </div>
        );
    }
  }, []);
  const addList = (type: any) => {
    if (list.length == 0) {
      setList([{ id: 0, type: type, completed: false }]);
    } else if (list.length > 4) {
      message.error('最多添加20条内容');
    } else {
      list.push({ id: list[list.length - 1].id + 1, type: type, values: [] });
      setList([...list]);
    }
  };

  const DragHandle = SortableHandle(() => (
    <span className={styles.dragHandle}>{<img src={MenuIcon} />}</span>
  ));
  const SortableItem = SortableElement(({ value }) => (
    <div>
      <DragHandle />
      <Button
        type="link"
        style={{
          fontSize: 14,
          lineHeight: '20px',
          height: '20px',
          margin: '0 0 5px 656px',
        }}
        onClick={(e) => {
          const ids = list.map((item) => item.id);
          list.splice(ids.indexOf(value.id), 1);
          setList([...list]);
        }}
      >
        删除
      </Button>
      {renderUpload(value)}
    </div>
  ));

  const Sortable = SortableContainer(({ children }) => {
    return <div>{children}</div>;
  });

  const SortableList = SortableContainer(({ items }) => {
    return (
      <div
        className="draggableUploadBox"
        style={{ display: 'flex', flexDirection: 'column', gap: '20px' }}
      >
        {list.map((value, idx) => {
          return <SortableItem key={`item-${value.id}`} index={idx} value={value} />;
        })}
      </div>
    );
  });

  return (
    <div className={styles.uploadBox}>
      <div style={{ display: 'inline-flex', gap: '10px', flexWrap: 'wrap', marginBottom: '20px' }}>
        <Button type="primary" onClick={() => addList('text')}>
          添加文本
        </Button>
        <Button type="primary" onClick={() => addList('image')}>
          添加图片
        </Button>
        <Button type="primary" onClick={() => addList('video')}>
          添加视频
        </Button>
        <Button type="primary" onClick={() => addList('file')}>
          添加文件
        </Button>
        <Button type="primary" onClick={() => addList('link')}>
          添加链接
        </Button>
        <Button type="primary" onClick={() => setOpenValidate(!openValidate)}>
          validate
        </Button>
      </div>
      <Sortable onSortEnd={onSortEnd} useDragHandle>
        <div
          className="draggableUploadBox"
          style={{ display: 'flex', flexDirection: 'column', gap: '20px' }}
        >
          {list.map((value, index) => (
            <SortableItem key={`item-${value.id}`} index={index} value={value} />
          ))}
        </div>
      </Sortable>
      <div
        className="draggableUploadBox"
        style={{ display: 'flex', flexDirection: 'column', gap: '20px' }}
      />
    </div>
  );
};

export default forwardRef((props, ref) => <UploadBox {...props} />);

```

```tsx
//new
import UploadFile from './UploadFile';
import UploadImg from './UploadImage';
import { Button, Image, message } from 'antd';
import React, { forwardRef, useCallback, useEffect, useRef, useState } from 'react';
import UploadVideo from './UploadVideo';
import UploadLink from './UploadLink';

import styles from './index.less';
import { deepClone } from '@/utils/utils';

const UploadBox: React.FC<any> = (props) => {
  const [valueList, setValueList] = useState({});
  const [list, setList] = useState<any>([
    {
      id: 2,
      type: 'image',
    },

    {
      id: 5,
      type: 'link',
    },
  ]);

  useEffect(() => {
    console.log('box中的value表', valueList);
  }, [valueList]);
  const renderUpload = useCallback((item: any) => {
    switch (item.type) {
      case 'file':
        return (
          <div
            key={item.id}
            style={{
              width: 700,
              borderRadius: '2px',
              border: '1px solid rgba(0,0,0,0.1500)',
              padding: '20px 12px',
            }}
          >
            <UploadFile />
          </div>
        );
      case 'link':
        return (
          <div
            key={item.id}
            style={{
              width: 700,

              borderRadius: '2px',
              border: '1px solid rgba(0,0,0,0.1500)',
              padding: '20px 12px',
            }}
          >
            <UploadLink
              onChange={(values: any) => {
                // valueList[item.id] = values;

                setValueList((pre) => ({ ...pre, [item.id]: values }));
              }}
            />
          </div>
        );
      case 'video':
        return (
          <div
            key={item.id}
            style={{
              width: 700,
              borderRadius: '2px',
              border: '1px solid rgba(0,0,0,0.1500)',
              padding: '20px 12px',
            }}
          >
            <UploadVideo />
          </div>
        );
      case 'image':
        return (
          <div
            key={item.id}
            style={{
              width: 700,
              borderRadius: '2px',
              border: '1px solid rgba(0,0,0,0.1500)',
              padding: '20px 12px',
            }}
          >
            <UploadImg
              onChange={(sourceUrl: string, mediaId: string) => {
                list.forEach((ele) => {
                  if (ele.id == item.id) {
                    ele.completed = true;
                    ele.mediaId = mediaId;
                    ele.url = sourceUrl;
                  }
                });
                setList([...list]);
              }}
              value={item.url}
            />
          </div>
        );
    }
  }, []);

  const addList = (type: any) => {
    if (list.length == 0) {
      setList([{ id: 0, type: type, completed: false }]);
    } else if (list.length > 4) {
      message.error('最多添加20条内容');
    } else {
      list.push({ id: list[list.length - 1].id + 1, type: type, values: [] });
      setList([...list]);
    }
  };

  return (
    <div className={styles.uploadBox}>
      <div style={{ display: 'inline-flex', gap: '10px', flexWrap: 'wrap', marginBottom: '20px' }}>
        <Button type="primary" onClick={() => addList('text')}>
          添加文本
        </Button>
        <Button type="primary" onClick={() => addList('image')}>
          添加图片
        </Button>
        <Button type="primary" onClick={() => addList('video')}>
          添加视频
        </Button>
        <Button type="primary" onClick={() => addList('file')}>
          添加文件
        </Button>
        <Button type="primary" onClick={() => addList('link')}>
          添加链接
        </Button>
      </div>

      <div
        className="draggableUploadBox"
        style={{ display: 'flex', flexDirection: 'column', gap: '20px' }}
      >
        {list.map((value, idx) => (
          <div>
            <Button
              type="link"
              style={{
                fontSize: 14,
                lineHeight: '20px',
                height: '20px',
                margin: '0 0 5px 656px',
              }}
              onClick={(e) => {
                const ids = list.map((item) => item.id);
                list.splice(ids.indexOf(value.id), 1);
                setList([...list]);
              }}
            >
              删除
            </Button>
            {renderUpload(value)}
          </div>
        ))}
      </div>
    </div>
  );
};

export default forwardRef((props, ref) => <UploadBox {...props} />);

```


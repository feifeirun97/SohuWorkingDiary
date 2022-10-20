## 基础篇

### 优点

+ 拥有依赖管理，动态打包，代码分离，按需加载，代码压缩，静态资源压缩，缓存配置
+ 拓展性强，自定义插件、loader
+ 社区庞大，资源多、更新快

### 基础应用

<img src="https://mmbiz.qpic.cn/mmbiz_png/Mpt86EGjlpsDCbrWliaOflGnJY4Pq8IB52It3ARhnbwD7K1gRobUJy6HMkC5omyl6nMGichhTqHW2I4P7IPMwZQQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1" style="zoom:50%;" />

+ webpack通过**关系依赖图**可以获取非代码资源，如images或web字体等
+ 每个模块都可以明确表述它自身的依赖，，避免打包未使用的模块

#### 入口entry

webpack从入口开始寻找依赖，构建依赖关系图，允许一个或多个入口配置

```js
//单入口
module.exports = {
	entry: './src/index.js'
}
//多入口
module.exports = {
	entry:{
    index: path.join(srcPath, 'index.js'),
  	other: path.join(srcPath, 'other.js'),
  }
}
```

#### 出口output

输出打包位置，文件名

```js
//单入口
module.exports = {
	output: {
    path: path.resolve(__dirname, 'dist'),
    filename: '[name].js'
  }
}
```

#### 加载器loader

Webpack自带js和json的打包能力，除此之外的文件类型如css、image、txt需要安装loader转换为有效模块。

```js
module.exports = {
  module: {
    rules: [
      {
        test: /\.css$/i,
        use: ["style-loader", "css-loader"],
      },
    ],
  },
};

```

## 实践

### 前置概念

webpack是**静态模块打包器**，本质就是打包js的引用

+ **loader**

Webpack自带js和json的打包能力，除此之外的文件类型如css、image、txt需要安装loader转换为有效模块打包

常见如`css-loader, sass-loader, babel-loader`

+ **plugin**

plugin可以在webpack运行到某个阶段时候，帮你做一些事情，类似react/vue中的生命周期。loader主要的职责是让webpack认识更多的文件类型，而plugin的职责则是让其可以控制构建流程。具体的某个插件（plugin）就是在webpack构建过程中的特定时机注入扩展逻辑来改变构建结果，作用于整个构建过程

+ **chunk**

代码块，一个chunk可能由多个模块组合，也用于代码合并和分割（这里的合并分割主要指指纹策略的判断，指纹策略简单来说就是文件名后的hash）

+ **chunks**

一个chunks至少包含一个chunk，chunks是chunk的合集。

下面代码中a.js就是chunk，而index.js就是chunks。**在webpack构建中入口是chunks，出口是chunk**（知道这个概念就行）

```js
// index.js  
import { a } from 'a.js'  
console.log('我是index文件')  

//a.js  
const a = '我是a文件'  
export { a }  
```

+ **bundle**

资源经过webpack流程解析编译后最终输出的成果文件（一个.js格式的文件，也就是我们的output文件）

- **entry**

文件打包的入口，webpack会根据entry递归的去寻找依赖，每个依赖都将被它处理，最后打包到集合文件中

- **output**

配置打包输出的位置、文件名等

- **mode**

目标环境，不用的目标环境会影响webpack打包时的决策

- - production：码进行压缩等一系列优化操作
  - development：有利于热更新的处理，识别哪个模块变化代
  - none：什么都不做，打包时会有提示警告

+ **babel**

js编译器，可以让我们不再考虑兼容性使用最新的js语法

+ **postcss**

用js和插件转换css代码的工具，类似babel，其本身作用不大，我们很多具体需求的实现都需要他的插件来完成，他本身更像一个平台

例如，项目中我们需要webpack自动的帮我们为css样式加上兼容性前缀，实际帮我们加上前缀的插件是`autoprefixer`[1]，但他能为我们加前缀的前提又是我们有postcss。**千万不要把postcss误解postcss成scss、less替代品**

+ **browserslist**

browserslist实际上就是声明了一段浏览器的合集，根据这个合集输出兼容性代码。应用于babel、postcss等工具当中。可以写在package.json中也可以写在.browserslistrc中

```js
// 在package.json中的写法  
// “> 1%”表示兼容市面上使用量大于百分之一的浏览，“last 1 chrome version”表示兼容到谷歌的上一个版本，具体的可以使用命令npx browserslist "> 1%"的方式查看都包含了哪些浏览器
{  
  "browserslist": ["> 1%", "last 2 versions"]  
} 
```

### 初始化项目

```bash
# 新建文件夹
mkdir learnWebpack
cd learnWebpack
# 默认配置创建 package.json 文件
npm init -y
# 用npm init -y后得到的内容 
{  
  "name": "learnwebpack",  
  "version": "1.0.0",  
  "description": "",  
  "main": "index.js",  
  "scripts": {  
    "test": "echo \"Error: no test specified\" && exit 1"  
  },  
  "author": "",  
  "license": "ISC"  
}  

# 安装webpack -d表示安装在开发环境[线上不需要]
npm install webpack@4.44.2 webpack-cli@3.3.12 -D
```

#### 创建目录

创建src文件夹，创建src/index.js文件`console.log('this is index.js')`

编辑package.json文件，添加start字段

```json
"scripts": {  
  "test": "echo \"Error: no test specified\" && exit 1",  
  "start": "webpack"  
} 
```

#### webpack的零配置

执行yarn start会发现编译成功。很显然我们这里什么都没配置，就帮我们成功打包了一个src下的代码。该功能实际上是wabpack默认帮我们配置了一套简单的打包配置，让我们看看webpack默认为我们配置了什么：

```js
//创建src和index文件是因为默认配置
const path = require('path')  
module.exports = {  
  extry: './src/index.js',  
  output: {  
    filename: 'main.js',  
    path: path.resolve(__dirname, './dist')   
  }  
}  
```

上面我们看到了，零配置很弱，真正的项目中他完全不可能满足我们的需求

我们想要自定义配置webpack的话，需要在根目录上创建一个`webpack.config.js`的文件，这个文件的内容可以覆盖webpack的零配置

```bash
# 存在webpack.config.ts文件时，输入该命令启动打包
webpack
# 通过--config可以指定配置文件启动打包
webpack --config anotherWebpackConfig.js
```

### 配置webpack.config.js

> 此文目标不是搭建一个完整全面的项目工程，目标是以一些有代表性的功能作为切入点学习webpack从而做到举一反三

#### 实现加载css

对于css文件我们需要一个loader加载器将它正确的打包进bundle文件。

在index文件中import引入index.css文件。安装css-loader包后配置webpack.config.js

打包完检查dist/main.js发现css内容成功打包了。

```bash
npm i css-loader@4.2.2 -D
touch webpack.config.js
yarn start
```

```js
const path = require('path')  
module.exports = {  
  entry: './src/index.js',  
  output: {  
    filename: 'main.js',  
    path: path.resolve(__dirname, './dist')  
  },  
  mode: "development",  //有利于热更新的目标环境
  module: {  
    rules: [  
      {  
        test: /\.css$/,  //$表示换行
        use: "css-loader"  
      }  
    ]  
  }  
}   
```

#### 实现css展示

> 此时如果你在dist文件夹下创建一个html文件并加上css类，并引入main.js文件，你会发现样式不生效。
>
> 原因是css只是被当作字符串引入了js中[相当于JSON.strinfufy],自然看不到效果。
>
> 所以我们需要style-loader帮我们把css内容放进style标签，如下图

⚠️某种文件使用多个loader时，use接受数组并且**从右向左执行**，所以style-loader要卸载css-loader前面

```bash
yarn add style-loader@1.3.0 -D
```

```js
module: {  
    rules: [  
      {  
        test: /\.css$/,  //$表示换行
        use: ['style-loader', 'css-loader']  //从右向左执行
      }  
    ]  
 }  
```

![图片](https://mmbiz.qpic.cn/mmbiz/XP4dRIhZqqUJj3vtys4fGFlKZtKaRrwfQNJX0wsgeZ1mPDo3opc7yJqQtdOuRb4PscUukUZwBqhKbxfyHn5rJw/640?wx_fmt=jpeg&wxfrom=5&wx_lazy=1&wx_co=1)

#### 实现css前缀自动补充

> css3在浏览器中会存在兼容性问题，我们可以通过手动给属性加上前缀来解决。autoprefixer插件也可以帮我们完成

已知autoprefixer是postcss工具的插件，所以我们需要安装postcss和postcss-loader

```bash
yarn add autoprefixer postcss-loader@4.2.0 postcss -D   
```

```js
module: {  
    rules: [  
      {  
        test: /\.css$/,  //$表示换行
        use: [
          'style-loader',
          'css-loader'，
          {  
            loader: 'postcss-loader',  
            options: {  
              postcssOptions: {  
                plugins: [require('autoprefixer')]  
              }  
            }  
          }  
        ]  //从右向左执行
      }  
    ]  
 }  
```

> 当loader需要写配置的时候，我们可以把loader写成一个对象，loader属性就是要使用的loader名称，options属性就是这个loader的配置对象。autoprefixer是postcss的插件，所以autoprefixer的使用自然就要写在postcss-loader的配置中了
>
> 因为postcss有自己的配置文件，所以这里还可以写成这样：

```js
module: {  
  rules: [  
    {  
      test: /\.css$/,  
      use: ['style-loader', 'css-loader', 'postcss-loader']  
    }  
  ]  
}
// 根目录下新建postcss.config.js文件  
module.exports = {  
  plugins: [require('autoprefixer')],  
}    
```

> 这里我们需要配置一下browserslist，否则插件不知道按照什么样的规则进行前缀补全

```js
// 在package.json文件中添加  
// 这里的意思表示目标浏览器为ie浏览器，并需要兼容到ie8以上  
"browserslist": ["ie > 8"]    
```

![图片](https://mmbiz.qpic.cn/mmbiz/XP4dRIhZqqUJj3vtys4fGFlKZtKaRrwfUDbNNyBHCnb3JOeENDMBaLcAduPrD0H4e61gNjD6cpovo4Jy5VCibbQ/640?wx_fmt=jpeg&wxfrom=5&wx_lazy=1&wx_co=1)
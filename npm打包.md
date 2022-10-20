### Step 1: npm account

```bash
#https://www.npmjs.com/signup
npm adduser
npm login
```

### Step2: Make a Package

> package name: tiny

```bash
md tiny
cd tiny && touch package.json
```

#### edit package.json

> package.json [name, version] is required 

It’s called a [**scoped package**](https://docs.npmjs.com/misc/scope). It allows us to use short names that might already be taken, for example the [**tiny** package](https://www.npmjs.com/package/tiny) already exists in npm.

The versioning system npm using is called [**SemVer**](https://semver.org/), which stands for **Semantic Versioning**

![semver.jpg](https://s2.loli.net/2022/10/18/txBMJ2yTGzYmUWK.jpg)

```json
{
  "name": "@z5239235/tiny",
  "version": "1.0.0",
  "description": "Removes all spaces from a string",
  "license": "MIT",
  "repository": "feifeirun97/tiny",
  "main": "index.js",
  "keywords": [
    "tiny",
    "npm",
    "package",
    "feifeirun97"
  ]
}

```

```bash
vim package.json
npm publish --access=public
```

到此为止，打包发布完成。但是很显然如果我们需要写入代码和适用是肯定不够的。所以我们需要创建一个repo。

### Step3：create a repository

维护一个readme，[shields](https://shields.io/category/build)添加版本信息

[![npm (scoped)](https://img.shields.io/npm/v/@z5239235/tiny)](https://github.com/feifeirun97/tiny)

[![npm bundle size](https://img.shields.io/bundlephobia/min/tiny)](https://github.com/feifeirun97/tiny)

[![npm bundle size](https://img.shields.io/bundlephobia/min/@z5239235/tiny)](https://github.com/feifeirun97/tiny)

```markdown
[![npm (scoped)](https://img.shields.io/npm/v/@z5239235/tiny)](https://github.com/feifeirun97/tiny)
[![npm bundle size](https://img.shields.io/bundlephobia/min/@z5239235/tiny)](https://github.com/feifeirun97/tiny)
this is the tiniest
```

### Step4: add a license

github repo => insights => community standards => license[add] => MIT License

```bash
npm version major
npm publish
```

[![npm (scoped)](https://camo.githubusercontent.com/ca0d153ba3cb1a2743bf2dd9b60104cc286f013790d491a54dc0890838d1985f/68747470733a2f2f696d672e736869656c64732e696f2f6e706d2f762f4062616d626c65686f7273652f74696e792e737667)](https://www.npmjs.com/package/@bamblehorse/tiny)











## 好处

最近在项目业务中有遇到一些问题，一些通用的方法或者封装的模块在PC、WAP甚至是APP中都需要使用，但是对于业务的PC、WAP、APP往往是不同的业务、不同的代码库中，尽管已经将公用的组件和方法抽离到各自公共common中，但是各个大业务大方向上的公用封装依然不能满足需求。

比如一个计算文档类型大小的方法，可能都同时存在于各个业务的common中，假设是有3处代码库中均有；如果此时的需求是将文档类型或者大小的方法进行一些修改，增加一种文档类型或者减少一种文档类型，那咱们是否是需要去共同修改上面的3处方法。这样做，很不利于代码的维护，浪费人力，增加了代码工作量。

那么，如何做到管理一些公共依赖的基础模块代码呢？这时候，封装发布一个npm包进行统一管理就是一个很好的办法了。


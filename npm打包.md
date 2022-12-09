## Npm发包流程

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



```js
module.exports = function tiny(string) {
  if (typeof string !== 'string') throw new TypeError('Tiny wants a string!');
  return string.replace(/\s/g, '');
};
//index.js代码
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












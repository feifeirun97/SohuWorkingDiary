# Npm常用指令与发包流程

## Npm简介

> npm （ Node Package Manager）是Node 的模块管理器，功能极其强大。 它是Node 获得成功的重要原因之一。 正因为有了npm，我们只要一行命令，就能安装别人写好的模块。

## 常用指令

```js
npm init --yes(初始化配置)  -y
npm i (会根据package.json里面的键dependencies，devDependencies来安装相对应的包)
npm i 包(默认安装一个最新的包，这个包在node_modules文件夹里面，并且会更新在你的package.json文件)
npm i 包@3.0.0（安装一个指定版本的包，会更新在你的package.json文件）
npm i 包 --save-dev(安装一个开发环境所需要的包，会更新在你的package.json文件) -D
npm i 包 --save(安装一个生产环境所需要的包，会更新在你的package.json文件) -S
npm uninstall 包（卸载一个包，会更新在你的package.json文件）
npm update 包(更新此包版本为最新版本，会更新在你的package.json文件)
npm run 脚本键(会根据package.json里面的"scripts"里面的脚本键自动执行相对于的值)
npm login 登陆npm账号
npm publish  (根据package.json的name发布一个包) 发布到npm仓库
npm unpublish 包名 --force(卸载npm网站上自己上传的包)
```

## 语意化版本

版本格式：主版本号.次版本号.修订号，版本号递增规则如下：

1. 主版本号：当你做了不兼容的 API 修改，
2. 次版本号：当你做了向下兼容的功能性新增，
3. 修订号：当你做了向下兼容的问题修正。

先行版本号及版本编译信息可以加到“主版本号.次版本号.修订号”的后面，作为延伸。

![semantic versioning for UT | UBports Forum](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAVAAAACWCAMAAAC/8CD2AAAA8FBMVEX///8AAABEcsT/wABwrUftfTJLdsX7+/vCwsLtfTP09PTi4uIqKir4+Pjq6urZ2dkYGBitra2hoaG8vLwyMjL/uwAxZ8A6bMJkpzNrqz//whj/y0/t8+mkyI/d4/KdsNyTk5PtgTt0r05mZmZvb29eXl7MzMykpKSzs7M/Pz+EhIR8fHyMjIxLS0vLy8tNTU05OTkiIiIRERFXV1f98+351cH+6rzsdR7R5MXF0ezvkln75tz/yDj/7cyNvW5kic7/4J73zbbrbgB+m9Xui0you+H/0mrE3LbV5sr/3I6fxYZboyH/9Nr/xSywz5yHodd9IEPSAAAQiElEQVR4nO1daaOruJFFyUwgLBJgul+/9OseEvbFC3Y6nelMMlk620yS+f//ZqoksAFjDBjfd19fnS8XI6uQDkdSqaRrKYqEhISEhISEhITEqwBNC7JZnHsX2oQQ+6QtNWA5WzBAvNyYlY3GGmJDlz73OaBaXmF1ltJheKTBcdlLcc8GyGkGOUGTyVn01KeA7srDsS7WQkI3pA13gYWwbaBiU7Ml5zwrEMqcjbWClahqVWUZoTHpIp1tIe8a8PSJ+coVCcVabB+2YnVrsohQavYIPU5WWI20Z4BEszM+TujpgSZ6gbYCoYXIm/mpG4nL0zwD1BbZgiQt6854akdcNu3rcUK9VcywxwmtTYieM15iRzRcT+i6bv1T8+psY6/ChGpeavEI9ryNhpvNYkLFQOvXn4TkD3MMUDEkNu6SsDe9HzZeF6GK4ztYFbaUUEGHd/4s+NjNsJB026zoAMzJ2V8boTUWE+r0MgpDxQwL3KG3L59FDzB5YFuJUOW1EMrHoap1Yz+rDwRCjr2aGJ0+5H7+M6FU88Mo9NMbcy2quUUUlk485G3qOifUt/QG11+JN0kewAM2U172YkKPfUGKFjx9trPrC1LlrySYml8Qmiq7wG7G1mygB9b252QzjHtpW88Uvp9tnuGFrRdj7fJty2EfekAPSwmlV0NIPNOSf6Vo7oeZU2ctgtAyIG1kPRGxrJNMoraKe67Oxci5CK2pdY278+OlhF7nY8emCU5DcUWooHjqbMmw+5W9esftSMFAun+D0EuzCa/TvDuMLiVUNNh2EzJ400gmW+DS6gzq7hqEtktQDqVfGHVuEdrIWN0OJIbjxVpKqHZFKJ05XPJRreO4OssJrbJLT3euzEWBR681TT7rz9oP81men3F+I0cz85pI0nic8SMT6rXvLCX0wKNFqlbr6Vg3yl2dvE25SZrUpB4uvfQudRz+JgK4qJG2x0kopJmnsco/MfEAf7RYr5jQvDIHcE4/E+r2DNS+h1UPKNedZrdbmlFsEVIab/OvmNCBEaGd3hDaHoU2rUZZX3eGfRGlMtuNYNZMiff7447dJ09od26WXyQYXdGtNK5Fu7r3CTV2MG/Yn4Lc2Z3eAKFdL0Y0yj1eiVEq7EKw3G60dwi1NvtjrwDPIVT0+O1YyCK3qT137btNkwjth6TPzpjR56GFtm8xTig7XOd+DqEDjj2/Mz38du3YcyfleJmmsEGoTbI9xARXuU2v12faaLeLUUIHPdXnECqmnu150dypp3tFKJekN2/q2Z+ZiWGHXZymIVStUWmM0GHP/zmEiuBIu8EJgqavH169AIu7eZMDgILQfheTNgq9NVHn6dMIbZmoMvPchTyJ0KDPHw9DHKcbEITk/aJMHtVE/j7/vNsw1SZebR+8AWTTCK0nUnZSD3x0t30ioWKYv7Q4IbhyJEcfp94rEZ3q5PhfPcr3mgS/l+GViDPdnXfdJrQWaLsNhE8kVBduSTNEiPj79Hi70nR3Z4mKkuwnZ68JzTs3y4vKRRd0d2FaEOoPpCRXfKqr+KGUscFwgJjHFZ1PwxsGYLgeEoouXJLal7WEoqZ7Cc0idFtc9Y4S/lprvynoPZqyuNsIzD7vhp8d0Gjek7iqZWN9KGUbgCbeY6nhh0HiXHigXajXCVRItMTi6XXgcWiNjoZQM29owV0MoraGxg3RYU1eozPipBklAiaavcVqz7V+yfXM/eA2BOo0Lk9YnY5+Oq8V+snwWEtM1KkQuXXDbWJ5W40N9EsDoder9oOo92gMBQRqE2aYh7X7O9i+RFdJ4uuUJty4L4om7j6x71G7cfptkftl0VS4qqurn6OZUYjpwTl63wka1vs1osL38yJqIi75uQ+tIHMe9iL/V30uHeRzQGJNmHFAouqpl7kaUnjjYA+5Q1euzZ3g7RlJP2Mb51dnVDe+0fZDb/hXvtLa4jeAvkZvub1XKxhNwtBoQ3sx80F5NZt+Brfp9Ig5DLy2QQyG4q9Lwfq7rwbLkg98o8KS0FsvZKCu6o3vXfHWSH2wXt2XO9xcm+9c9yaIzrLOfmpsubePso3uko86KLL+OHUdNPBFwgijVxOY/nKgKM310DHgjLWgF+e80S0Hsv7KDbJ2l3LM2WxwHpEcrb3u4105CaxPaZBcF3TTCaSELRY66s2Nxlg2MCIo6jWGis7CgxndHit0N4+yU+GP+OPOycyK2+JjZbHPgjyd2txrXIpM3WLr2d4+dAe9YD2FEpqVt4VS3txBrpXRwayyKE9644Duhll1NLeFzy4PnVnUPqx783Pr3gP0OxbUR3cQq5Y+WgjrTjr/jn6jmJiwwhZnCQkJCQkJiU8Pv/z+30fx7W/uGfivH4/is9/eLcPv3v/bGN7/dTS3+t8/+uj4/OJZS0J/iISO8ikJlYR+dELHm/wX9wh997H57BL68xYEiZ079wn9WRuCxfaNKQr9RQuCxfaNX98h9Ec/eQCCkUcsAN7dCvf8FPn86V0GRvAlV+UDBj58ga38q0fKMAdffw50vvuPZ5mXhK4MSejKkISuDEnoypCErgxJ6MqQhK4MSejKkISuDEnoypCErgxJ6MqQhK4MSejKkISuDEnoypCErgxJ6MqQhK4MSejKkISuDLGM/IgFSWiN33yDEArll/fW46/wj18h+LaHz/jlr+Za+DXgb3/FXQ/vf/c3/PBhroU5UP8T8WfcJfHuT/z66zXN/+GP3wLq3SKAP/5hroXffoaot4sg/j7Xwv98ARC7SN7j5f8+lVDlT58DxK6Td3j5+eT/jJoC9bvuFpzv5v+3yF+6e3B+9su5Bnhrb+1penK75639grXb/TffdrY0fTPfwj8+6+xp+v18C191Njm9f65AQaKdTVE/WVWggI5Ev1tioSPRf84WKEj0/QsKtCfRd39e23xboksE2pXoEoH2JPpsgXYlurpAOxJdJNDaZ1ouUJBoS6D/WlaGOfj63RMF2pboMoGCRB8UaFuiv3i+QNsSfYJAFeW89/b7pRa+fEygLYm+hEBBos8U6EWiSwUKvuiDAr1I9EUEepHoUwSqKM0O5uUWvlzqgzb48JICPUv0OQJtJLpcoI1Elwv0LNGXEehZos/6j+3vH+pBEcIXfcDAh5cUaD3QP0ugQqKPCFQM9I8IVFH+9f4FJkkX/B8y+uCvMozgu8U+aIO/PCZQPl16OYFyiT5PoCjRxwSKEn1MoCjRFxQoSvTd8wQKEn1QoCjRBw18+OIFBYoz+icKFCT6oEBhoP/9oxa+ekmBgkSf44O+XTyzwUtISEhISEhISEhISEhISLx6sJ34oWFDa/3gcDx+XGUbaryjtSGq6LslizN0B+g98hKxMIZ+M3oJDO3qCGpLsXYjsSa6WxA4ieozOwLiX24ep59krpsio+WRjbK7c3DoMFxSVXb3IIDi8svZ5YxDwMfgE/PkdU/QKRPFGDxho0Y6+VzRFoIjz0Ur4l9uGtPP3dYPNj83iREg1GJLYmEJ0XWdOq2zVKzW7+D7M45lGoFPHEtR0w6jxFVUNtKmNosI3fLfe0/NrY8myhKrsoFmpqZ5AvaslPpjp8boXslPECgCIJQ6lpXqWunwUqY5HmVCE70cP9UjERo8BPiz6KUfo2ZRooaPGX1bdf05JwkNwiA+/+tCG9rFrMSfht+QQNNTuLCS0oGqqi7UP8WiG26Z0KWEBj62+ZOPhG6PwAs096rAplyYQBUl1eDhHw10M8GcuqfxJk8p2VYB2YMaDqQ4kBike7hzMFJNKDyU2l6+B6VGJPOhOqeA5CAt+7QlQwfczMGm7owocRQoVr4lVMmJ5xtQQp3YRXU04G/kRXiUBTCd25BhGaGRAXwZNuNVsET1vFwJ8VDIKFPp8MEdZ+im40CjjE0dKh0joRGeLsKguJAaeha7e/JXQnY7LY0gT4nHcxWmokKTtzAf8OBjL3caL8R9lPUJs/qhgJKB+qI9b/JIaIRP3W+B0AKPK2F4H4qQLiT0pMBLc07qwcePTAs5oaqHR6twxY2LAxSqQhmCUm0IxcYKxczQHiMGGzrspwNxqsqWjw4W25xMBV8OrTXl44vxtw8uVBT1AYy6VygFnky0ARnVhPLOWyOU99yUF5fuEpIsJrTcK4cUCVVDQk4RJ1Tnox+rCRqBbrrKwbcqZrUIhVLpxw03wNjdk+US0hwAQffEjraC0KY2fFB6mNCk3eRzXjLjTCjW0CBQg1gQqh2JFx6XEwq1tqkFhPqE6Vg9rlA83kSbRqhz2GSK3iW0pdD7hDZX+4xaSl5xW1hjxXL0dQg16uNafLDKFZraSkehG1SoIFQlpa7qDxCqkCxUkNATdCb6HjpS6EMLE9rIaatOIZTiGNIntMTShAdrDqHYiRl2Bf2no1jHEt+osQ6hUBwYvy3uNmEfqu7Bp4APSGiQwRC630MNBKEMW2cKbC8i9LTFh4ELaZY4roZZAD0iEEqzKswqQ7lL6NHFUxINTmjT51Iokr63w63NhNJG4Z4J9UkQZieCBgsgMwqJLxz7cvJxVTdREi/y+ElOuW2GmWfgZCZE8mjlhYcDbQjdKZ4dRnsvXObYM3DxKB6th3M/5jpM0SgSqlgwU0MfbTcuMBXzGbGqqJBP1yx1xzNhSTTfgWu4d6cIxuWV7WB2aEEGusGhLXVjPIBcWWf+aaSlmHoWAXNTLKC+2ehYUqwq+qEav9bBG/Y1FZ7LeVkF9pwzTj85FHfP91wZzBub4H76CB91aufCcNcK8LxOsB929SQkJCSeisHTrgegt4KGRt+ZnhEy/sHj1sGkfTittYKodxa5ZfqrleeTx/UZxcNoExr0XC01m3OU6ycKTWcuTrqYwTAAv3PPgUeYDzk4I0z40aecUJgs8RkcTV2+IMFccSyqlZ6dRk4oc7jNINITcXwrdfCLb4JQkpGCBOC4b/Gg7D0Jj8e6o8NDriuYmp/22NyR0JAEHgYZUrIPcEXCJdEW0xgxA7SBQELBYMRt2iQ4YpgnJVmEa1FvgdBjVcc7kSnftlBXImWHQSYe5tEwDOoAV8BNWSnWMRHBeYxWJidF5VGMerkQCGW4XoYLGSFeeCG3z+++BUL5YBNGSuHB323AYuYSShljKl/edgmLY+2YIqH7E6SmGMZTKNPgRnRMcExnJIWErVjRFn0ojX2iiz40JeqGaHHMiP82CEVh5R4PflDTPHjewaMnaO1xzAVJ+B0XCT3Y/DpWtOwIbdxR9MImZqrEBO4eDiHvLZFQ17QPJyQUOdaI6nMjhzdCKA42qFDo86xDqeo6ZYoBQxQolPKwq67rho7fy0JI1ZlqkFzcoOB1BkRnJLbgO6LrBUI3xKEW5uadByg0PRq6bjH6NvpQaJbUTjihYsWhzIQPzwll2IdiQBn4848GEqbi8izwlOC6g2IQQ68wAl35PBcQmqOVAnKHNliArsDAZmCQzZtQqE1Opbm1RHiOemYZNSu2nFAlJ0VBIhX7Wmtr57gcQKtDGZowZJUkLM0IB6Co9I5nhe7gY7SHlxCQQ7lHKz4Jc7JXVc//SLV8QZBkkzu49iAWpNPivOGECp9z5+fIMI/bb8oSv0bdHHpOHP7LkrNvJEVST00NEGOMW2lSxnebiPuxW/Bl54c3s7x+TJ1SSkwEeQPd2ovCeQOtUEJCQkJCQuKj4P8BWhGjEj/i5NgAAAAASUVORK5CYII=)

## 发包流程

> 发布一个最基础的npm package需要的流程

### 1.登陆npm

```js
#https://www.npmjs.com/signup
npm adduser
npm login
```

### 2.新建仓库

npm包大致目录结构

包名 ├── dist/index.js ├──.npmignore ├── CHANGELOG.md ├── README.md └── package.json

+ `README文件`

当前项目的说明文档

+ `CHANGELOG文件`

当前项目的升级改版说明文档, 每次升级版本时, 都要附加升级说明

+ `.npmignore文件`

类似于.gitignore文件, 该文件中的目录会被npm忽略, 不被上传

+ `.package.json文件`

项目的配置文件

### 3.package.json配置

```json
{
	"name": "template-package", //发布的包名，默认是上级文件夹名。不得与现在npm中的包名重复。包名不能有大写字母/空格/下滑线!
  "version": "1.0.0",//你这个包的版本，默认是1.0.0。对于npm包的版本号有着一系列的规则，模块的版本号采用X.Y.Z的格式，具体体现为：
  //1、修复bug，小改动，增加z。
  //2、增加新特性，可向后兼容，增加y
  //3、有很大的改动，无法向下兼容,增加x
  "description": "用来请求接口的npm包", // 介绍
  "main": "index.js",// 入口文件，默认是Index.js，可以修改成自己的文件,这个很重要，当你在实际项目使用的时候，let a = require("包名")，它就去会去找对应的文件路径哦。
  "scripts": {                  // 快捷命令，在package.json同目录下输入命令 npm run 键 就会执行 相对应的命令
    "bulid": "npx webpack --config myConfig.js",
  	"start": "xxx",
  	//例如 输入 npm run bulid 就会执行npx webpack --config myConfig.js的命令 。
  	// npm start 省略run
  },
  "keywords": [],
  "author": "许飞",
  "license": "ISC",// 这个直接回车，开源文件协议吧，也可以是MIT，看需要吧。
  "dependencies": {             // 生产环境所依赖的包
    "jquery": "^3.4.1",
    "sea": "^1.0.2"
  },
  "devDependencies": {          // 开发环境所依赖的包
    "webpack": "^4.41.6"
  }
}
```

### 4.Readme维护

维护一个readme，[shields](https://shields.io/category/build)添加版本信息

[![npm (scoped)](https://img.shields.io/npm/v/@z5239235/tiny)](https://github.com/feifeirun97/tiny)

[![npm bundle size](https://img.shields.io/bundlephobia/min/tiny)](https://github.com/feifeirun97/tiny)

[![npm bundle size](https://img.shields.io/bundlephobia/min/@z5239235/tiny)](https://github.com/feifeirun97/tiny)

```markdown
[![npm (scoped)](https://img.shields.io/npm/v/@z5239235/tiny)](https://github.com/feifeirun97/tiny)
[![npm bundle size](https://img.shields.io/bundlephobia/min/@z5239235/tiny)](https://github.com/feifeirun97/tiny)
this is the tiniest
```

### 5.发布

```

```


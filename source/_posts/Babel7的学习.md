title: Babel7的学习
author: coolsong
date: 2020-08-21 19:28:50
tags:
  - Babel
categories:
  - Babel
---
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;之前对于babel的了解，主要停留在它对于代码的转换。而对于其配置项的作用并不是很了解，主要是通过cli自己创建或者网上copy的。最近看了一些关于babel7的学习文章，于是自己也就跟着学习一边，下面就是一些相关知识的总结：
<!--more-->
#### 什么是Babel

>总的来说Babel是一个JS编译器，主要用于将ECMAScript 20·5+版本的代码转换为向后兼容的JavaScript语法，以便能够运行在当前和旧版本的浏览器或其他环境中。

>babel能做什么:
* 语法转换
* 通过 Polyfill 方式在目标环境中添加缺失的特性(@babel/polyfill模块)
* 源码转换(codemods)

>下面通过一些小demo以及一些相应的配置，搞清楚实际中的使用和配置，以及@babel/runtime，@babel/polyfill，@babel/plugin-transform-runtime这些的作用。

#### @babel/core 和@babel/cli
* @babel/core

>模块包含了Babel的核心功能。不安装@bable/core，无法使用babel进行编译。

* @babel/cli
>bable提供的命令行工具，主要是提供babel这个命令

##### 试一试
> 新建一个babel项目，使用npm init -y进行初始化。创建一个src目录，再在里面创建一个index.js， 文件内容如下：

```JavaScxript
const alertName = () => {
  console.log('sxy');
}
```
>安装相应的依赖

```
cnpm install --save-dev @babel/core @babel/cli
```
>在package.js添加相应的命令

```
"compiler": "babel src --out-dir lib --watch"
```
* 运行命令npm run compiler, 生成了相应的文件，但是和开始我们所敲的代码完全一致（由于我们没有配置任何的插件）下面就来介绍一下插件。

#### 插件

>Babel 构建在插件之上，使用现有的或者自己编写的插件可以组成一个转换通道，Babel 的插件分为两种: 语法插件和转换插件。

* 语法插件

>这些插件只允许 Babel 解析（parse） 特定类型的语法（不是转换），可以在 AST 转换时使用，以支持解析新语法。

* 转换插件

>转换插件会启用相应的语法插件(因此不需要同时指定这两种插件)。

##### 具体使用

>如果插件发布在 npm 上，可以直接填写插件的名称， Babel 会自动检查它是否已经被安装在 node_modules 目录下，在项目目录下新建 .babelrc 文件 (下文会具体介绍配置文件)，配置如下（配置一个转换箭头函数的插件）：

```Json
{
    "plugins": ["@babel/plugin-transform-arrow-functions"]
}
```
>安装插件

```
cnpm install @babel/plugin-transform-arrow-functions -D
```
> 再运行命令之后，发现已经成功的进行了转化。
> 现在我们仅仅对箭头函数进行了处理，要想处理其他的特性，我们还需要安装相应的插件进行配置。如果一个一个的加会非常麻烦和好费时间。那我们想直接配置一个就将所有的特性都进行转化呢？
> 这就需要用到预设preset

#### 预设

* 通过使用或创建一个preset即可轻松使用一组插件。

>官方Preset
>* @babel/preset-env
>* @babel/preset-flow
>* @babel/preset-react
>* @babel/preset-typescript
>从 Babel v7 开始，所有针对标准提案阶段的功能所编写的预设(stage preset)都已被弃用，官方已经移除了 @babel/preset-stage-x

##### @babel/preset-env

>@babel/preset-env 主要作用是对我们所使用的并且目标浏览器中缺失的功能进行代码转换和加载 polyfill，在不进行任何配置的情况下，@babel/preset-env 所包含的插件将支持所有最新的JS特性(ES2015,ES2016等，不包含 stage 阶段)，将其转换成ES5代码。例如，如果你的代码中使用了可选链(目前，仍在 stage 阶段)，那么只配置 @babel/preset-env，转换时会抛出错误，需要另外安装相应的插件。

```Json
{    
    "presets": ["@babel/preset-env"]
}
```
>需要说明的是，@babel/preset-env 会根据你配置的目标环境，生成插件列表来编译。对于基于浏览器或 Electron 的项目，官方推荐使用 .browserslistrc 文件来指定目标环境。默认情况下，如果你没有在 Babel 配置文件中(如 .babelrc)设置 targets 或 ignoreBrowserslistConfig，@babel/preset-env 会使用 browserslist 配置源。如果你不是要兼容所有的浏览器和环境，推荐你指定目标环境，这样你的编译代码能够保持最小。

* 例如 仅包括市场份额超过0.2%的用户所需的polyfill和代码转化（ie版本大于等于9，并且依然有安全更新的浏览器）

```Json
>0.2%
not dead
ie >= 9
```
>继续刚才的例子， 我们添加.browserlistrc、安装@babel/preset-env

```Json
last 2 Chrome versions

cnpm install @babel/preset-env -D
```
>在进行编译 你会发现箭头函数不会被编译成ES5，因为 chrome 的最新2个版本都能够支持箭头函数。现在，我们将 .browserslistrc 仍然换成之前的配置。
>就针对我们目前的代码来说是能够进行正常转换了

* 我们再修改下src/index.js

```JavaScript
const isHas = [1,2,3].includes(2);
const p = new Promise((resolve, reject) => {
    resolve(100);
});
```
* 编译之后
```JavaScript
"use strict";
var isHas = [1, 2, 3].includes(2);
var p = new Promise(function (resolve, reject) {
  resolve(100);
});
```
> 我们发现includes方法并没有被转换，而它是es6中扩展的数组方法，一些低版本浏览器不支持。同时promise低版本也不支持。
> 产生这种问题的原因是语法转换只是将高版本的语法转换成低版本的，但是新的内置函数、实例方法无法转换。这时，就需要使用 polyfill 上场了，顾名思义，polyfill的中文意思是垫片，所谓垫片就是垫平不同浏览器或者不同环境下的差异，让新的内置函数、实例方法等在低版本浏览器中也可以使用。


#### Polyfill

>@babel/polyfill 模块包括 core-js 和一个自定义的 regenerator runtime 模块，可以模拟完整的 ES2015+ 环境（不包含第4阶段前的提议）。
>这意味着可以使用诸如 Promise 和 WeakMap 之类的新的内置组件、 Array.from 或 Object.assign 之类的静态方法、Array.prototype.includes 之类的实例方法以及生成器函数(前提是使用了 @babel/plugin-transform-regenerator 插件)。为了添加这些功能，polyfill 将添加到全局范围和类似 String 这样的内置原型中(会对全局环境造成污染，后面我们会介绍不污染全局环境的方法)。
>V7.4.0 版本开始，@babel/polyfill 已经被废弃(前端发展日新月异)，需单独安装 core-js 和 regenerator-runtime 模块。

* 首先安装@babel/polyfill 

```
cnpm install --save @babel/polyfill
```
* 需要在代码之前加载，修改index.js

```JavaScript
import '@babel/polyfill';
const isHas = [1,2,3].includes(2);
const p = new Promise((resolve, reject) => {
    resolve(100);
});
```
* 我们也可以在webpack中配置
```
entry: [    require.resolve('./polyfills'),    path.resolve('./index')]
```
* polyfills.js 文件内容如下
```JavaScript
import '@babel/polyfill';
```
>现在，我们的代码不管在低版本还是高版本浏览器(或node环境)中都能正常运行了。不过，很多时候，我们未必需要完整的 @babel/polyfill，这会导致我们最终构建出的包的体积增大.
>我们还是更希望能够动态的引入polyfill。这时我们需要增加配置项
>
>@babel/preset-env 提供了一个 useBuiltIns 参数，设置值为 usage 时，就只会包含代码需要的 polyfill 。有一点需要注意：配置此参数的值为 usage ，必须要同时设置 corejs (如果不设置，会给出警告，默认使用的是"corejs": 2) ，注意: 这里仍然需要安装 @babel/polyfill(当前 @babel/polyfill 版本默认会安装 "corejs": 2):
>
>使用 core-js@3 的原因，core-js@2 分支中已经不会再添加新特性，新特性都会添加到 core-js@3。例如你使用了 Array.prototype.flat()，如果你使用的是 core-js@2，那么其不包含此新特性。为了可以使用更多的新特性，建议大家使用 core-js@3。

* 安装依赖

```
cnpm install --save core-js@3
```

* 修改 .babelrc 同时去掉index.js中引入的polyfill
```JavaScript
{
  "presets": [
    [
      "@babel/preset-env",
      {   
        "useBuiltIns": "usage",
        "corejs": 3
      }
    ]
  ]
}
```
* 再次编译之后
![Image](/images/babel1.png)

> 实现了动态引入
> Babel会使用一些辅助函数来实现，默认情况下 会被注入到相应的文件中，下面是一个例子：

* 转换前

```JavaScript
const isHas = [1,2,3].includes(2);
const p = new Promise((resolve, reject) => {
    resolve(100);
});
setTimeout(async () => {
  await Promise.resolve('1')
  console.log('ok')
}, 1000)
```
* 转换后

```JavaScript
"use strict";
require("core-js/modules/es.array.includes");
require("core-js/modules/es.object.to-string");
require("core-js/modules/es.promise");
require("regenerator-runtime/runtime");
function asyncGeneratorStep(gen, resolve, reject, _next, _throw, key, arg) { try { var info = gen[key](arg); var value = info.value; } catch (error) { reject(error); return; } if (info.done) { resolve(value); } else { Promise.resolve(value).then(_next, _throw); } }
function _asyncToGenerator(fn) { return function () { var self = this, args = arguments; return new Promise(function (resolve, reject) { var gen = fn.apply(self, args); function _next(value) { asyncGeneratorStep(gen, resolve, reject, _next, _throw, "next", value); } function _throw(err) { asyncGeneratorStep(gen, resolve, reject, _next, _throw, "throw", err); } _next(undefined); }); }; }
var isHas = [1, 2, 3].includes(2);
var p = new Promise(function (resolve, reject) {
  resolve(100);
});
setTimeout( /*#__PURE__*/_asyncToGenerator( /*#__PURE__*/regeneratorRuntime.mark(function _callee() {
  return regeneratorRuntime.wrap(function _callee$(_context) {
    while (1) {
      switch (_context.prev = _context.next) {
        case 0:
          _context.next = 2;
          return Promise.resolve('1');
        case 2:
          console.log('ok');
        case 3:
        case "end":
          return _context.stop();
      }
    }
  }, _callee);
})), 1000);
```
>这样的话 有一个问题就是，所有使用到的文件都会注入一些相同的方法，导致包的体积增大。
>
>这时就需要使用@babel/plugin-transform-runtime 插件。所有帮助程序都将引用模块 @babel/runtime，这样就可以避免编译后的代码中出现重复的帮助程序，有效减少包体积。

#### @babel/plugin-transform-runtime
>@babel/plugin-transform-runtime 是一个可以重复使用 Babel 注入的帮助程序，以节省代码大小的插件。另外，@babel/plugin-transform-runtime 需要和 @babel/runtime 配合使用。

* 安装依赖

```
npm install --save-dev @babel/plugin-transform-runtime
npm install --save @babel/runtime
```
>@babel/plugin-transform-runtime 通常仅在开发时使用，但是运行时最终代码需要依赖 @babel/runtime，所以 @babel/runtime 必须要作为生产依赖被安装。
>除了前文所说的，@babel/plugin-transform-runtime 可以减少编译后代码的体积外，我们使用它还有一个好处，它可以为代码创建一个沙盒环境，如果使用 @babel/polyfill 及其提供的内置程序（例如 Promise ，Set 和 Map ），则它们将污染全局范围。虽然这对于应用程序或命令行工具可能是可以的，但是如果你的代码是要发布供他人使用的库，或者无法完全控制代码运行的环境，则将成为一个问题。

* 修改配置文件
```JavaScript
{
  "presets": [
    [
      "@babel/preset-env",
      {   
        "useBuiltIns": "usage",
        "corejs": 3
      }
    ]
  ],
  "plugins": [
    [
        "@babel/plugin-transform-runtime"
    ]
  ]
}
```
* 重新编译之后
![Image](/images/babel2.png)

>可以看到辅助函数现在是从@babel/runtime中引入的。


> 前文说了使用 @babel/plugin-transform-runtime 可以避免全局污染，我们来看看是如何避免污染的。但是从代码中可以看到会直接在原型链上增加相应的方法，会污染原型链。拿要想实现相应的效果，我们还需要对插件进行相应的配置。

* 首先新增依赖 @babel/runtime-corejs3:
```
cnpm install @babel/runtime-corejs3 -S
```
* 修改配置文件如下(移除了 @babel/preset-env 的 useBuiltIns 的配置
```
{
  "presets": [
    [
      "@babel/preset-env"
    ]
  ],
  "plugins": [
    [
        "@babel/plugin-transform-runtime",{
          "corejs": 3
        }
    ]
  ]
}
```
> 重新编译之后

![Image](/images/babel3.png)

>可以看到没有直接去修改原型方法 ，而是通过引入相应的方法来实现相应的效果。

#### 参考链接

>[不容错过的 Babel7 知识](https://juejin.im/post/6844904008679686152)






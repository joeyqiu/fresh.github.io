## Guides

The initial issue was that unlike server languages, there was no way to guarantee that every user has the same support for JavaScript because users could be using different browsers with varying levels of support (especially older versions of Internet Explorer). If developers wanted to use new syntax (e.g. `class A {}`), users on old browsers would just get a blank screen due to the `SyntaxError`.

Babel provided a way for developers to use the latest JavaScript syntax while allowing them to not worry about how to make it backwards compatible for their users by translating it (`class A {}` to `var A = function A() {}`).

Babel 是一个工具链，主要用于将 ECMAScript 2015+ 版本的代码转换为向后兼容的 JavaScript 语法，以便能够运行在当前和旧版本的浏览器或其他环境中。

Babel可以做的事情：

* Transform syntax，语法转换
* Polypill features that are missing in your target environment(through: @babel/polyfill), 通过 Polyfill 方式在目标环境中添加缺失的特性
* Source code transformations(codemods), 源码转换
* And more! (...)

其实主要是语法解析，一些新的API（Promise，Set，Map）的实现都需要垫片（babel-polyfill）



"preset"  is just a pre-determined set of plugins，为了避免解析的时候一个一个插件的特别多，所以把一些通用插件集成起来放在preset下面.

这边有个通用的preset插件‘env’

```
npm install --save-dev @babel/preset-env
```



### polyfill

@babel/polyfill模块包含了core-js和一个自定义的regenerator runtime来模仿所有es2015+环境。

以前都是一次性把@babel/polyfill直接引入，但是这个垫片的文件尺寸还是蛮大的，所以最好是按需引入。



或者如果你确切的知道所需要的polyfills功能，可以直接从`core-js`获取它们。

```
import 'core-js/es/map';
import 'core-js/es/set';
```

现在使用'env' preset，包含了一个"useBuiltIns"选项，设为“usage”后就会自动按需引入。

```
const presets = [
  [
    "@babel/env",
    {
      targets: {
        edge: "17",
        firefox: "60",
        chrome: "67",
        safari: "11.1",
      },
      useBuiltIns: "usage",
    },
  ],
];

module.exports = { presets };
Babel会检测代码，如果在目标浏览器环境中不支持该语法，就会引入对应的polyfill
```



### Configure Babel

现在有两种配置文件: `babel.config.js`和`.babelrc`



* you want to programmatically create the configuration?
* you want to compile 'node_modules'?

```
babel.config.js is for you
```



* you have a static configuration that only applies to your simple single package?

```
.babelrc is for you
```



`env` preset 已经出来一年多了，用来完全取代如下的presets

* Babel-preset-es2015
* babel-preset-es2016
* babel-preset-es2017
* babel-preset-latest
* A combination of the above



最重要的改变，就是把所有的包都切换到指定作用域的: `babel-cli` => `@babel/cli`








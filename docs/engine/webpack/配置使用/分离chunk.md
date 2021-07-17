## 分离chunk

常用的代码分离方法有三种：

* 入口起点：使用`entry`配置手动地分离代码
* 防止重复：使用`SplitChunkPlugin`去重和分离chunk
* 动态导入：通过模块中的内联函数调用来分离代码





代码分割，动态载入的优点:

- 不需要下载所有的代码，节省宽带
- 按需载入，可能某些功能永远不会触发，对应的脚本代码也就不用下载了
- ...(more)

当涉及到动态代码拆分的时候，webpack提供了两个类似的技术：

- ES6的import()语法
- webpack的遗留功能，特定的require.ensure()方法。

```
如果使用的是import()语法，在内部会用到promise，旧版本的浏览器需要加个promise-polyfill的垫片。
```

### webpack与babel的协作

webpack是内置支持动态载入的，但是与babel（用于把JSX转成javascript）协作使用的时候，会需要使用到`@babel/plugin-syntax-dynamic-import`插件。这只是一个语法插件，意味着Babel并不会添加额外的转换。 这个插件允许Babel解析动态载入的部分，方便webpack用代码分割的方式来打包。`.babelrc`的配置一般如下：

```
{
  "presets": ["@babel/preset-react"],
  "plugins": ["@babel/plugin-syntax-dynamic-import"]
}
```

新版的babel中，`plugin-syntax-dynamic-import`已经修改成`babel-plugin-proposal-dynamic-import`了，然后在该插件中，可以看到如下代码提示：

```
If you are using Webpack or Rollup and thus don't want
Babel to transpile your imports and exports, you can use
the @babel/plugin-syntax-dynamic-import plugin and let your
bundler handle dynamic imports.
```

这么看来，babel和它的插件，是为了方便webpack来实现动态载入的方式，所以使Babel不对`import`和`export`的方法进行转码。

### babel对import和export的转译

import的转译

```
import tool from './tool.js'
```

=>

```
"use strict";

var _tool = _interopRequireDefault(require("./tool.js"));

function _interopRequireDefault(obj) {
  return obj && obj.__esModule ? obj : { default: obj };
}
```

export的转译

```
export default {};
```

=>

```
"use strict";

exports.__esModule = true;
exports["default"] = void 0;
var _default = {};
exports["default"] = _default;
```

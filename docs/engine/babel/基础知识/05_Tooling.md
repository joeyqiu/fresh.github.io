### @babel/core

```
var babel = require("@babel/core");
import { transform } from "@babel/core";
import * as babel from "@babel/core";
```

在这边得到transform方法，

```
babel.transform(code, options, function(err, result) {
  result; // => { code, map, ast }
});
```

但是按照parser之后transform，觉得应该接受的一个语法树，然后导出最终的代码。

```
const sourceCode = "if (true) return;";
const parsedAst = babel.parse(sourceCode, { parserOpts: { allowReturnOutsideFunction: true } });
babel.transformFromAst(parsedAst, sourceCode, options, function(err, result) {
  const { code, map, ast } = result;
});
```

### @babel/parser

Babel解析器（以前是Bbabylon）是Babel使用的一个JavaScript语法解析器。

主要是基于`acorn`和`acorn-jsx`两个库。

解析器基于Babel的AST格式，会产出一个AST（抽象语法树）。AST for JSX code is based on Facebook JSX AST.

### @babel/generator

把AST转成code。

### @babel/core-frame

Generate errors that contain a code frame that point to source locations. code-frame是用来指出源码中错误位置的帮助库。

### @babel/runtime

是一个包含Babel模块运行时的帮助库，也是`regenerator-runtime`的一个版本（是这个意思么？） This is meant to be used as a runtime dependency along with the Babel plugin `@babel/plugin-transform-runtime`. Please check out the documentation in that package for usage.

### @babel/traverse

We can use it alongside the babel parser to traverse and update nodes

### @babel/types

类型检测吧，巨多API

### @babel/template

In computer science, this is known as an implementation of quasiquotes.

### cli安装

cli安装的命令如下：

```
npm install --save-dev @babel/core @babel/cli
```

cli是因为命令行中执行需要，core中自带上述各种工具。@babel/core的依赖如下：

```
"dependencies": {
    "@babel/code-frame": "^7.10.4",
    "@babel/generator": "^7.11.4",
    "@babel/helper-module-transforms": "^7.11.0",
    "@babel/helpers": "^7.10.4",
    "@babel/parser": "^7.11.4",
    "@babel/template": "^7.10.4",
    "@babel/traverse": "^7.11.0",
    "@babel/types": "^7.11.0",
    "convert-source-map": "^1.7.0",
    "debug": "^4.1.0",
    "gensync": "^1.0.0-beta.1",
    "json5": "^2.1.2",
    "lodash": "^4.17.19",
    "resolve": "^1.3.2",
    "semver": "^5.4.1",
    "source-map": "^0.5.0"
},
```

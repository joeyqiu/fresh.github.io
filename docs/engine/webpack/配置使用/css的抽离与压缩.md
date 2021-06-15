## CSS的抽离与压缩

webpack打包的时候，生产环境，一般都会把css单独抽离出来，然后压缩一下。这边用到的两个plugin如下：

* mini-css-extract-plugin (把css代码抽离出单独文件)
* optimize-css-assets-webpack-plugin (压缩css代码)



### 没有使用mini-css-extract-plugin的时候

mini-css-extract-plugin有个loader的，如果没有mini-css-extract-plugin的时候，css代码不会被加载到html中去，所以需要加个`style-loader` 来加载css代码。

```
require.resolve('style-loader')
```



在生成的js代码中，包含着css的代码

```
// EXTERNAL MODULE: ./src/index.css
var src = __webpack_require__(32);
```

在id为32的模块中

```
(function(module, exports, __webpack_require__) {
  var content = __webpack_require__(33);
  if(typeof content === 'string') { content = [[module.i, content, '']];}
  var transform;
  var insertInto;
  var options = {"hmr":true}
  options.transform = transform
  options.insertInto = undefined;
  var update = __webpack_require__(19)(content, options);
  if(content.locals) module.exports = content.locals;
  if(false) {}
/***/ }),
```

在这边`__webpack_require__(33)` 里面，把css代码push到css-loader中去

```
(function(module, exports, __webpack_require__) {

// 请求css-loader的代码
exports = module.exports = __webpack_require__(18)(false);

// Module
exports.push([module.i, "body {\n  margin: 0;\n  font-family: -apple-system, BlinkMacSystemFont, \"Segoe UI\", \"Roboto\", \"Oxygen\",\n    \"Ubuntu\", \"Cantarell\", \"Fira Sans\", \"Droid Sans\", \"Helvetica Neue\",\n    sans-serif;\n  -webkit-font-smoothing: antialiased;\n  -moz-osx-font-smoothing: grayscale;\n}\n\ncode {\n  font-family: source-code-pro, Menlo, Monaco, Consolas, \"Courier New\",\n    monospace;\n}\n", ""]);
/***/ })
```

这边的`__webpack_require__(19)` 中会把所有的css都创建成style标签，然后插入到head中去



### 使用mini-css-extract-plugin之后

会把css代码抽离到单独的文件中，然后在js同级目录下创建一个css的文件夹，用来放抽离出来的css文件。

index.js中通过`import './index.css';`引入的css文件

```
// EXTERNAL MODULE: ./src/index.css
var src = __webpack_require__(30);


/***/ 30:
/***/ (function(module, exports, __webpack_require__) {
// extracted by mini-css-extract-plugin
/***/ }),
这边就直接说明了，通过mini-css-extract-plugin插件，把css代码抽离出来了
```

然后通过`html-webpack-plugin`插件，来把这个css文件插入到html的head中

```
<link href="/static/css/main.85b607a5.chunk.css" rel="stylesheet" />
```





### css的压缩

使用的是`optimize-css-assets-webpack-plugin`插件，在webpack的optimization配置下，具体配置如下

```
const safePostCssParser = require('postcss-safe-parser');
{
	optimization: {
		minimize: true,
		minimizer: [
			xxx,
			new OptimizeCSSAssetsPlugin({
          cssProcessorOptions: {
            parser: safePostCssParser,
            map: shouldUseSourceMap
              ? {
                  // `inline: false` forces the sourcemap to be output into a
                  // separate file
                  inline: false,
                  // `annotation: true` appends the sourceMappingURL to the end of
                  // the css file, helping the browser find the sourcemap
                  annotation: true,
                }
              : false,
          },
        })
		]
	}
}
```

设置`minimize` 为true，用来告知webpack使用`TerserPlugin` 来压缩bundle。

然后``minimizer``的设置就是允许你通过提供一个或多个定制过的`TerserPlugin`实例，覆盖默认的压缩工具

这边的cssProcessorOptions用来设置并指定css的处理器来优化\压缩css文件，这边指定的css处理器是`safePostCssParser`。 默认的处理器是`cssnano`。



### postcss-safe-parser

这是一个对postcss有较好容错性的解析器，能够找到遗留的语法错误并修复，适合于遗留代码。


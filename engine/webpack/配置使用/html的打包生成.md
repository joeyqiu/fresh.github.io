## html的打包生成

html这边都是通过`html-webpack-plugin` 来进行打包处理的。

这个插件对于在文件名中包含每次会随着编译而发生变化哈希的webpack bundle尤其有用。你可以让插件为你生成一个html文件，使用`loadsh模版` 提供你自己的模版，或者用你自己的loader



在plugins中进行配置

```
plugins: [
	// Generates an `index.html` file with the <script> injected.
      new HtmlWebpackPlugin(
        Object.assign(
          {},
          {
            inject: true,
            template: paths.appHtml,
          },
          isEnvProduction
            ? {
                minify: {
                  removeComments: true,
                  collapseWhitespace: true,
                  removeRedundantAttributes: true,
                  useShortDoctype: true,
                  removeEmptyAttributes: true,
                  removeStyleLinkTypeAttributes: true,
                  keepClosingSlash: true,
                  minifyJS: true,
                  minifyCSS: true,
                  minifyURLs: true,
                },
              }
            : undefined
        )
      ),
]
```

这边会提供html模版文件，就放在`paths.appHtml` 也就是`public`目录下的`index.html` 文件。

这边通过插件的`monify` 属性来进行压缩配置。



把webpack生成的runtime脚本直接插入到html中去，因为生成的这个js比较小，不值得浪费一次ajax请求。

这边的插件是在`react-dev-utils`中提供的。

```
const InlineChunkHtmlPlugin = require('react-dev-utils/InlineChunkHtmlPlugin');

isEnvProduction &&
        shouldInlineRuntimeChunk &&
        new InlineChunkHtmlPlugin(HtmlWebpackPlugin, [/runtime~.+[.]js/]),
```





### 模版中的变量

这边提供的模版中，会有`%PUBLIC_URL%` 这样的变量

```
<link rel="manifest" href="%PUBLIC_URL%/manifest.json" />
```

配置如下：

```
new InterpolateHtmlPlugin(HtmlWebpackPlugin, env.raw)
```



这边的变量在打包时候获取值，是通过`react-dev-utils/InterpolateHtmlPlugin` 这个插件提供的。

这个变量的使用说明如下，

```
This Webpack plugin lets us interpolate custom variables into `index.html`.
Usage: `new InterpolateHtmlPlugin(HtmlWebpackPlugin, { 'MY_VARIABLE': 42 })`
Then, you can use %MY_VARIABLE% in your `index.html`.
```


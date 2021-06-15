## css代码的解析

现在还是习惯用less的预编译，sass在编译的时候会引入`node-sass`，然后就经常会出错，所以我现在比较常用less预编译工具。



### 使用到的loader

配置中提供的css相关的loader函数如下

```
const getStyleLoaders = (cssOptions, preProcessor) => {
    const loaders = [
      isEnvDevelopment && require.resolve('style-loader'),
      isEnvProduction && {
        loader: MiniCssExtractPlugin.loader,
        options: shouldUseRelativeAssetPaths ? { publicPath: '../../' } : {},
      },
      {
        loader: require.resolve('css-loader'),
        options: cssOptions,
      },
      {
        // Options for PostCSS as we reference these options twice
        // Adds vendor prefixing based on your specified browser support in
        // package.json
        loader: require.resolve('postcss-loader'),
        options: {
          // Necessary for external CSS imports to work
          // https://github.com/facebook/create-react-app/issues/2677
          ident: 'postcss',
          plugins: () => [
            require('postcss-flexbugs-fixes'),
            require('postcss-preset-env')({
              autoprefixer: {
                flexbox: 'no-2009',
              },
              stage: 3,
            }),
            // Adds PostCSS Normalize as the reset css with default options,
            // so that it honors browserslist config in package.json
            // which in turn let's users customize the target behavior as per their needs.
            postcssNormalize(),
          ],
          sourceMap: isEnvProduction && shouldUseSourceMap,
        },
      },
    ].filter(Boolean);
    if (preProcessor) {
      loaders.push({
        loader: require.resolve(preProcessor),
        options: {
          sourceMap: isEnvProduction && shouldUseSourceMap,
        },
      });
    }
    return loaders;
  };
```

loader的顺序依次如下：

```
sass-loader/less-loader => postcss-loader => css-loader => xxx

css-loader之后需要判断下环境，如果是开发环境，则使用style-loader，把css代码放在js文件中，运行的时候直接创建style标签。

如果是生产环境，则使用mini-css-extract-plugin插件的loader，用来把css代码抽离出来，并且单独放在css文件夹下面的css文件
```



### 各自的作用

* less-loader:  加载和转译LESS文件
* sass-loader: 加载和转译SASS/SCSS文件
* postcss-loader: 使用PostCSS加载和转译CSS/SSS文件
* css-loader: 解析CSS文件后，使用import加载，并且返回CSS代码
* style-loader: 将模块的导出作为样式添加到DOM中



### module中的配置

在module配置下，设置了css代码的解析

```
// style files regexes
const cssRegex = /\.css$/;
const cssModuleRegex = /\.module\.css$/;
const lessRegex = /\.(less|css)$/;
const lessModuleRegex = /\.module\.(less|css)$/;
```



解析.css文件，并且过滤掉css moudle部分的文件

```
{
  test: cssRegex,
  exclude: cssModuleRegex,
  use: getStyleLoaders({
    importLoaders: 1,
    sourceMap: isEnvProduction && shouldUseSourceMap,
  }),
  sideEffects: true,
},
```



解析css module的文件

```
{
              test: cssModuleRegex,
              use: getStyleLoaders({
                importLoaders: 1,
                sourceMap: isEnvProduction && shouldUseSourceMap,
                modules: true,
                getLocalIdent: getCSSModuleLocalIdent,
              }),
            },
```



解析sass部分的代码，过滤css module部分

```
{
              test: sassRegex,
              exclude: sassModuleRegex,
              use: getStyleLoaders(
                {
                  importLoaders: 2,
                  sourceMap: isEnvProduction && shouldUseSourceMap,
                },
                'sass-loader'
              ),
              sideEffects: true,
            },
```



解析css module的sass部分

```
{
              test: sassModuleRegex,
              use: getStyleLoaders(
                {
                  importLoaders: 2,
                  sourceMap: isEnvProduction && shouldUseSourceMap,
                  modules: true,
                  getLocalIdent: getCSSModuleLocalIdent,
                },
                'sass-loader'
              ),
            },
```



解析less部分，过滤css module的less代码

```
{
              test: lessRegex,
              exclude: lessModuleRegex,
              use: getStyleLoaders(
                {
                  importLoaders: 2,
                  sourceMap: isEnvProduction && shouldUseSourceMap,
                },
                'less-loader'
              ),
              sideEffects: true,
            },
```



解析css module的less部分

```
{
              test: lessModuleRegex,
              use: getStyleLoaders(
                {
                  importLoaders: 2,
                  sourceMap: isEnvProduction && shouldUseSourceMap,
                  modules: true,
                  getLocalIdent: getCSSModuleLocalIdent,
                },
                'less-loader'
              ),
            },
```


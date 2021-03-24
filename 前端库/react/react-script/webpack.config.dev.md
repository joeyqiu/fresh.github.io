## webpack.config.dev.js

```
const isEnvDevelopment = webpackEnv === 'development';
const isEnvProduction = webpackEnv === 'production';

用这两个变量来控制是开发环境还是生产环境
```



### entry

src/index.js



### output

```
pathinfo: true
// tell webpack to include comments in bundles with information about the contained modules. default true in development and false in production mode respectively.
```

```
filename: 'static/js/bundle.js'
// this option determines the name of each output bundle. the bundle is written to the directory specified by the 'output.path' option
// this does not produces a real file, it's just the virtual path that is served by WebpackDevServer in development. 
```

```
chunkFilename: 'static/js/[name].chunk.js'
// this option determines the name of non-entry chunk files
// there are also additional JS chunk files if you use code splitting.
```

```
publicPath: '/'
// this is the url that app is served from.  specifies the public URL of the output directory when referenced in a browser.
// this is an important option when using on-demand-loading(按需加载) or loading external reoueces like images, files, etc. If an incorrect value is specified you'll receive 404 errors while loading these resources.
```

```
devtoolModuleFilenameTemplate: info =>
      path.resolve(info.absoluteResourcePath).replace(/\\/g, '/')

// this option is only used when devtool uses an options which requires module names.
// customize the names used in each source map's sources array. this can bbe done by passing a template string or function.
// Point sourcemap entries to original disk location(format as URL on windows)
```



### optimization

since version4 webpack runs optimizations for you depending on the chosen `mode`, still all optimizations are available for manual configuration and overrides.

```
splitChunks: {
  chunks: 'all',
  name: false,
}

chunks(and modules imported inside them) were connected by a parent-child relationship in the internal webbpack graph. the 'CommonnsChunkPlugin' was used to avoid duplicated dependencies across them, but further optimizations were not possible.
打包通常是根据wbepack的父子关系图表来组织的，但是以前通过CommonsChunkPlugin可以提供避免重复的问题
webpackV4+之后。用optimization.splitChunks 来取代 CommonsChunkPlugin
```

```
chunks: 'all'
when a string is provided, valid values are 'all', 'async', and 'initial', all是非常强大的， chunks can be shared even between async and non-async chunks.
```

```
name: false
the name of the split chunk. Providing true will automatically generate a name based on chunks and cache group key.
```



```
runtimeChunk: true,
Setting optimization.runtimeChunk to true or "multiple" adds an additional chunk to each entrypoint containing only the runtime.
```



### resolve

A resolver is a library which helps in locating a module by its absolute path. 通过绝对路径来帮助定位模块

The dependency module can be from the application code or a third party library. The resolver helps webpack find the module code that need to be included in the bundle for every such `require/import` statement. webpack uses `enhanced-resolve` to resolve file paths while bundling modules.

resolver帮助webpack找到需要打包到bundle中的代码。



```
modules: ['node_modules'].concat(
 // It is guaranteed to exist because we tweak it in `env.js`
  process.env.NODE_PATH.split(path.delimiter).filter(Boolean)
 // 在node_modules和node的变量环境路径下面去查找依赖包
),
tell webpack what directories should be searched when resolving modules.
```

```
extensions: paths.moduleFileExtensions
  .map(ext => `.${ext}`)
  .filter(ext => useTypeScript || !ext.includes('ts'))

Automatically resolve certain extensions. Which is what enables users to leave off the extension when importing.
通过import或者require导入文件的时候，可以省略扩展名

const moduleFileExtensions = [
  'web.mjs',
  'mjs',
  'web.js',
  'js',
  'web.ts',
  'ts',
  'web.tsx',
  'tsx',
  'json',
  'web.jsx',
  'jsx',
];
ts和tsx两个扩展名，需要判断useTypeScript变量
```



```
alias: {
  'react-native'(别名): 'react-native-web'(依赖的库),
}
create aliases to import or require certain modules easily.
```

 instead of using relative paths when importing like so:

```js
import Utility from '../../utilities/utility';
```

you can use the alias:

```js
import Utility from 'Utilities/utility';
```



```
plugins: [
  PnpWebpackPlugin,
  new ModuleScopePlugin(paths.appSrc, [paths.appPackageJson]),
]
A list of additional resolve plugins which should be applied. It allows plugins such as 'DirectoryNameWebbpackPlugin'.

ModulesScopePlugin： 防止用户从src(或node_modules)目录之外引入文件，
```



### resolveLoader

this set of options is identical to the `resolve` property set above, but is used only to resolve webpack's loader packages, 

该属性与`resolve`属性效果相等。但只用于查找webpack的加载包。



### module

these options determine how the different types of modules within a project will be treated.

项目中不同类型的模块，该如何处理



```
strictExportPresence: true
// makes missing exports an error instead of warning.
把缺少exports当作一个错误，而不是警告.  比较严格
```



### Rule

A Rule can be separated into three parts: Condition, Result and nested Rules.

```
{ parser: { requireEnsure: false } },
// disable require.ensure

Rule.parser

An object with parser options, All applied parser options are merged.
解析的选项
```



````
{
    test: /\.(js|mjs|jsx)$/,
    enforce: 'pre',
    use: [
      {
        options: {
          formatter: require.resolve('react-dev-utils/eslintFormatter'),
          eslintPath: require.resolve('eslint'),
          // @remove-on-eject-begin
          baseConfig: {
            extends: [require.resolve('eslint-config-react-app')],
            settings: { react: { version: '999.999.999' } },
          },
          ignore: false,
          useEslintrc: false,
          // @remove-on-eject-end
        },
        loader: require.resolve('eslint-loader'),
      },
    ],
    include: paths.appSrc,
}

Rule.test
is a shortcut to Rule.resource.test, If you apply a Rule.test option, you cannot also supply a Rule.resource.

Rule.enforce(possible value: 'pre' | 'post')
specifies the category of the loader, No value means normal loader.
````

#### Rule.test
is a shortcut to `Rule.resource.test`, If you apply a `Rule.test` option, you cannot also supply a Rule.resource.

`{test: Condition}` : The condition must match, The convention is to provide a RegExp or array of RegExps here, but it's not enforced.



#### Rule.enforce(possible value: 'pre' | 'post')
specifies the category of the loader, No value means normal loader.
There are two phases that all loaders enter one after the other:

* Pitching phase:  post , inline, normal, pre
* Normal phase: pre, normal, inline, post

用来处理loader的执行顺序。



#### Rule.use

A list of `UseEntries`which are applied to modules, Each entry specifies a loader to be used.

```
loader: require.resolve('eslint-loader'),
表示用的eslint-loader
```

#### Use Entry

it must have a `loader` property being a string, It is resolved to the configuration `context` with the loader resolving options(resolveLoader)

It can have an `options` property being a string or object. This value is passed to the loader, which should interpret it as loader options.



#### Rule.include

is a shortcut to `Rule.resource.include`. If you supply a `Rule.include` option, you cannot also supply a `Rule.resource`.

`{include: Condition}`: The condition must match. The convention is to provide a string or array of string here, but it's not enforced.



### Rule.oneOf

An array of `Rules` from which only the first matching Rule is used when the Rule matches.

When no loader matches it will fall back to the loader at the end of the loader list.



```
{
            test: [/\.bmp$/, /\.gif$/, /\.jpe?g$/, /\.png$/],
            loader: require.resolve('url-loader'),
            options: {
              limit: 10000,
              name: 'static/media/[name].[hash:8].[ext]',
            },
          }
```

`url` loader works like `file` loader except that it embeds assets smaller than specified limit in bytes as data URLs to avoid request. A missing `test` is equivalent to a match.





```
{
            test: /\.(js|mjs|jsx|ts|tsx)$/,
            include: paths.appSrc,
            loader: require.resolve('babel-loader'),
            options: {
              customize: require.resolve(
                'babel-preset-react-app/webpack-overrides'
              ),
              // @remove-on-eject-begin
              babelrc: false,
              configFile: false,
              presets: [require.resolve('babel-preset-react-app')],
              // Make sure we have a unique cache identifier, erring on the
              // side of caution.
              // We remove this when the user ejects because the default
              // is sane and uses Babel options. Instead of options, we use
              // the react-scripts and babel-preset-react-app versions.
              cacheIdentifier: getCacheIdentifier('development', [
                'babel-plugin-named-asset-import',
                'babel-preset-react-app',
                'react-dev-utils',
                'react-scripts',
              ]),
              // @remove-on-eject-end
              plugins: [
                [
                  require.resolve('babel-plugin-named-asset-import'),
                  {
                    loaderMap: {
                      svg: {
                        ReactComponent: '@svgr/webpack?-prettier,-svgo![path]',
                      },
                    },
                  },
                ],
              ],
              // This is a feature of `babel-loader` for webpack (not Babel itself).
              // It enables caching results in ./node_modules/.cache/babel-loader/
              // directory for faster rebuilds.
              cacheDirectory: true,
              // Don't waste time on Gzipping the cache
              cacheCompression: false,
            },
          }
```

Process application JS with Babel, the preset includes JSX, Flow, and some ESnext feature



````
{
            test: /\.(js|mjs)$/,
            exclude: /@babel(?:\/|\\{1,2})runtime/,
            loader: require.resolve('babel-loader'),
            options: {
              babelrc: false,
              configFile: false,
              compact: false,
              presets: [
                [
                  require.resolve('babel-preset-react-app/dependencies'),
                  { helpers: true },
                ],
              ],
              cacheDirectory: true,
              // Don't waste time on Gzipping the cache
              cacheCompression: false,
              // @remove-on-eject-begin
              cacheIdentifier: getCacheIdentifier('development', [
                'babel-plugin-named-asset-import',
                'babel-preset-react-app',
                'react-dev-utils',
                'react-scripts',
              ]),
              // @remove-on-eject-end
              // If an error happens in a package, it's possible to be
              // because it was compiled. Thus, we don't want the browser
              // debugger to show the original code. Instead, the code
              // being evaluated would be much more helpful.
              sourceMaps: false,
            },
          }
````

Process any JS outside of the app with Babel.

Unlike the application JS, we only compile the standard ES features.



```
{
            test: cssRegex,
            exclude: cssModuleRegex,
            use: getStyleLoaders({
              importLoaders: 1, //  允许下面1个loader优先在css-loader之前执行
            }),
          }

const cssRegex = /\.css$/;
const cssModuleRegex = /\.module\.css$/;
```

`postcss` loader applies autoprefixer to our CSS.

`css` loader resolves path in CSS and adds assets as dependencies.

`style` loader turns CSS into JS modules that inject <style> tags



css-loader的`importLoaders`: allows you to configure how many loader before `css-loader` should be applied to `@import` ed resources. 



```
// common function to get style loaders
const getStyleLoaders = (cssOptions, preProcessor) => {
  const loaders = [
    require.resolve('style-loader'),
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
        ],
      },
    },
  ];
  if (preProcessor) {
    loaders.push(require.resolve(preProcessor));
  }
  return loaders;
};
```



```
{
            test: cssModuleRegex,
            use: getStyleLoaders({
              importLoaders: 1,
              modules: true,
              getLocalIdent: getCSSModuleLocalIdent,
            }),
          }
```

Adds support for CSS modules using the extension .module.css



```
{
  test: sassRegex,
  exclude: sassModuleRegex,
  use: getStyleLoaders({ importLoaders: 2 }, 'sass-loader'),
}
const sassRegex = /\.(scss|sass)$/;
const sassModuleRegex = /\.module\.(scss|sass)$/;
```

// opt-in support for SASS(using .scss or .sass extensions)



```
{
  test: sassModuleRegex,
  use: getStyleLoaders(
    {
      importLoaders: 2,
      modules: true,
      getLocalIdent: getCSSModuleLocalIdent,
    },
   'sass-loader'
  ),
}
```

// adds support for CSS Modules, but using SASS, using the extension .module.scss or .module.sass



```
{
  exclude: [/\.(js|mjs|jsx|ts|tsx)$/, /\.html$/, /\.json$/],
  loader: require.resolve('file-loader'),
  options: {
    name: 'static/media/[name].[hash:8].[ext]',
  },
}
```

js和html，json需要过滤，用webpack内部的loader，其他的如果都不识别，就用这边的file-loader





### Plugins

The `plugins` option is used to customize the webpack build process in a variety of ways. webpack comes with a varitey built-in plugins available under `webpack.[plugin-name]`

A webpack plugin is a Javascript object that has an `apply` method, This apply method is called by the webpack compiler, giving access to the entire compilation lifecyle.



````
new HtmlWebpackPlugin({
      inject: true,
      template: paths.appHtml,
    })
````

The `HtmlWebpackPlugin` simplifies creation of HTML files to serve your webpack bundles.

Generates an `index.html` file with the `<script>` injected.



```
new InterpolateHtmlPlugin(HtmlWebpackPlugin, env.raw)
```

Makes some environment variables available in index.html, the public URL is available as `%PUBLIC_URL%` in index.html, 



```
new ModuleNotFoundPlugin(paths.appPath)
```

gives some necessary context to module not found errors. such as the requesting resource.



```
new webpack.DefinePlugin(env.stringified)
```

Allow global constants configured at compile time. 使一些全局变量在编译时可用

// if (process.env.NODE_ENV === 'development') {...}



```
new webpack.HotModuleReplacementPlugin()
```

Enable Hot Module Replacement(HMR).

__HMR should never be used in production.__

It's necessary in development mode.



```
new CaseSensitivePathsPlugin()
```

watcher doesn't work well if you mistype casing in a path so we use a plugin that prints an error when you attempt to do this.

大小写错误的监听插件



```
new WatchMissingNodeModulesPlugin(paths.appNodeModules)
```

If you require a missing module and then `npm install` it. You still have to restart the development server for webpack to discover it. this plugin makes the discovery automatic so you don't have to restart.



```
new webpack.IgnorePlugin(/^\.\/locale$/, /moment$/)
```

prevent generation of modules for `import` for `require` calls matching the following regular expressions.

防止通过import或者require调用的模块，变成正则表达式中的



```
new ManifestPlugin({
    fileName: 'asset-manifest.json',
    publicPath: publicPath
})
```

Generate a manifest file which contains a mapping of all asset filenames to their corresponding output file so that tools can pick it up without having to parse `index.html`



### Node

these options configure whether to polypill or mock certain `Node.js` globals and modules. This allows code originally written for the Node.js environment to run in other environments like the browser.

```
node: {
    dgram: 'empty',
    fs: 'empty',
    net: 'empty',
    tls: 'empty',
    child_process: 'empty',
  }
```

Tell webpack to provide empty mock for them, so importing them works.



### Performance

these options allows you to control how webpack notifies you of assets and entry points that exceed a specific file limit. 



````
performance: false,
````

turn off performance processing because we utilize our own hints vai the FileSizeReporter.
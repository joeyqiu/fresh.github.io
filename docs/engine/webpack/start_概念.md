## Webpack

本质上，webpack是一个现代javascript应用程序的`静态模块打包器`(static module bundler)。在webpack处理应用程序时，会在内部创建一个依赖图，用于映射到项目需要的每个模块，然后将所有这些依赖生成到一个或多个bundle。

核心概念：

* 入口（entry）
* 输出（output）
* loader
* 插件（plugins）



## entry

__入口起点(entry point)__指示webpack应该使用哪些模块，来作为构建其内部依赖图的开始。webpack会找出有哪些模块和library是入口起点（直接或间接）依赖的。

默认值是`./src/index.js`。可以在配置文件中通过`entry`属性来制定一个不同的入口起点。



## output

output属性告诉webpack在哪里输出它所创建的bundles，以及如何命名这些文件，主输出文件默认为`./dist/main.js`，其他生成文件的默认输出目录是`./dist`。

你可以通过在配置文件中指定一个`output`字段，来配置这些处理过程。



## loader

作为开箱即用的自带特性，webpack自身只支持JavaScript。而loader能够让webpack处理那些非Javascript文件，并且先将它们转换为有效模块，然后添加到依赖图中，这样就可以提供给应用程序使用。

在更高层面，webpack的配置中loader有两个特征：

* `test`属性：用于标识出应该被对应的loader进行转换的某个或某些文件
* `use`属性：表示进行转换时，应该使用哪个loader。

loader支持链式传递。loader链中每个loader，都对前一个loader处理后的资源进行转换。loader链按照相反的顺序执行。第一个loader将（应用转换后的资源作为）返回结果传递给下一个loader，依次这样执行下去。最终，在链中最后一个loader，返回webpack所预期的Javascript.

Loader的配置一般都放在`module`的属性中。



## plugins

loader被用于转换某些类型的模块，而插件则可以用于执行范围更广的任务，插件的范围包括：打包优化、资源管理和注入环境变量。



loader用于加载资源，plugin用于优化管理



## Runtime/Manifest

在使用webpack构建的典型应用程序或站点中，有三种主要的代码类型：

1. 你或你的团队编写的源码
2. 你的源码会依赖的任何第三方的library或“vendor”代码。
3. webpack的runtime和manifest，管理所有模块的交互。



runtime以及manifeset主要是指：在浏览器运行时，webpack用来连接模块化的应用程序的所有代码。

### runtime

runtime主要包含：在模块交互时，链接模块所需的加载和解析逻辑。包括浏览器中的已加载模块的连接，以及懒加载模块的执行逻辑。



### manifest

当编译器(compiler)开始执行、解析和映射应用程序时，它会保留所有模块的详细要点。这个数据集合称为”Manifest“，当完成打包并发送到浏览器时，会在运行时通过Manifest来解析和加载模块。

无论你选择哪种模块语法，那些 import 或 require 语句现在都已经转换为 __webpack_require__ 方法，此方法指向模块标识符(module identifier)。通过使用 manifest 中的数据，runtime 将能够查询模块标识符，检索出背后对应的模块。



### Devtool

this option controls if and how source maps are generated.

Use the `SourceMapDevToolPlugin` for a more fine grained configuration. See the `source-map-loader` to deal with existing source maps.




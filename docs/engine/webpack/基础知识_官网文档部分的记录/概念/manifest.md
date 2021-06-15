## manifest

在使用webpack构建的典型应用程序或站点中，有三种主要的代码类型：

1. 你或者你的团队编写的源代码
2. 你的源码会依赖的任何第三方library或“vendor”代码。
3. webpack的runtime和manifest，管理所有模块的交互。



### runtime

Runtime，以及伴随的manifest数据，主要是指：在浏览器运行过程中，webpack用来连接模块化应用程序所需的所有代码。它包含：在模块交互时，连接模块所需要的加载和解析逻辑。包括：

* 已经加载到浏览器中的连接模块逻辑
* 以及尚未加载模块的延迟加载逻辑



### manifest

当compiler开始执行、解析和映射应用程序时，它会保留所有模块的详细要点。这个数据集合称为“manifest”，当完成打包并发送到浏览器时，runtime会通过manifest来解析和加载模块。无论你选择哪种模块语法，那些`import/require`语句现在都已经转换为`__webpack_require__`方法，此方法指向模块标识符(module identifier)。 通过使用manifest中的数据，runtime能够检索这些标识符，找出每个表示出背后对应的模块。



### 问题

通过使用内容散列(content hash)作为 bundle 文件的名称，这样在文件内容修改时，会计算出新的 hash，浏览器会使用新的名称加载文件，从而使缓存无效。一旦你开始这样做，你会立即注意到一些有趣的行为。即使某些内容明显没有修改，某些 hash 还是会改变。这是因为，注入的 runtime 和 manifest 在每次构建后都会发生变化。

查看管理输出指南的 manifest 部分，了解如何提取 manifest，并阅读下面的指南，以了解更多长效缓存错综复杂之处。



### 进一步阅读

- [分离 manifest](https://survivejs.com/webpack/optimizing/separating-manifest/)
- [使用 webpack 提供可预测的长效缓存](https://medium.com/webpack/predictable-long-term-caching-with-webpack-d3eee1d3fa31)
- [缓存](https://webpack.docschina.org/guides/caching/)


## pnp-webpack-plugin

使用方式比较简单，如下：

```
const PnpWebpackPlugin = require(`pnp-webpack-plugin`);
 
module.exports = {
  resolve: {
    plugins: [
      PnpWebpackPlugin,
    ],
  },
  resolveLoader: {
    plugins: [
      PnpWebpackPlugin.moduleLoader(module),
    ],
  },
};
```

`resolve` 入口会根据你的程序选取解析正确的依赖，然后`resolveLoader` 则会帮助webpack在你的硬盘上找到loader的位置。这种情况下，所有的loaders都会相对于你配置文件所在的目录去进行解析。

如果你的配置来自第三方包，并且使用它们的loaders。请确认它们使用`require.resolve` ， 这会确保解析进程是跨平台的，并防止一些未定义的行为。



使用pnp插件后，loader的引入方式如下：都是通过`require.resolve`方法

```
module.exports = {
  module: {
    rules: [{
      test: /\.js$/,
      loader: require.resolve('babel-loader'),
    }]
  },
};
```



没有使用pnp插件的时候，旧的使用方式如下：

```
module: {
    rules: [
        {
          test: /\.css$/,
          use: [
            'style-loader',
            'css-loader'
          ],
        },
     ]
}
```


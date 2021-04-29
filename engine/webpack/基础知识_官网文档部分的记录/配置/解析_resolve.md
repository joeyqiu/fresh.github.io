## 解析

设置模块如何被解析。webpack提供合理的默认值，但是还是可能会修改一些解析的细节。



### alias

创建`import`或`require`的别名，来确保模块引入变得更简单。

也可以在给定对象的键后的末尾添加 `$`，以表示精准匹配。



### extensions

```
[string] : ['.wasm', '.mjs', '.js', '.json']
```

自动解析确定的扩展。默认值如上



然后能使用户在引入模块时，不用带上扩展，比如file.js，可以直接引入。

```
import File from '../file';
```



### modules

```
[string]: ['node_modules']
```

告诉webpack解析模块时应该搜索的目录。

绝对路径和相对路径都能使用，但是两者之前有一点点区别。

相对路径将类似于Node查找`mode_modules`的方式进行查找。

使用绝对路径，将只在给定目录中搜搜。



modules数组指定搜索的顺序，下面制定先搜索绝对路径src目录，然后在搜索`node_modules`下面。可以依次添加，依次搜索。

```
modules: [path.resolve(__dirname, 'src'), 'node_modules']
```



### plugins

应该使用的额外的解析插件列表，它允许插件，如：`DirectoryNamedWebpackPlugin`。



### mainFields

当从npm包中导入模块时(如： `import * as D3 from 'd3'`)，此选项将决定在`package.json` 中使用哪个字段导入模块。根据webpack配置中指定的`target`不同，默认值也会有所不同。

所以也就是指定包的入口的字段名称么。。

默认值：

```
module.exports = {
  //...
  resolve: {
    mainFields: ['browser', 'module', 'main']
  }
};
```





## resolveLoader

这组选项与上面的`resolve`对象的属性集合相同，但仅用于解析webpack的loader包。默认值如下：

```
module.exports = {
  //...
  resolveLoader: {
    modules: ['node_modules'],
    extensions: ['.js', '.json'],
    mainFields: ['loader', 'main']
  }
};
```


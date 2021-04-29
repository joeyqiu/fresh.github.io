## General

### Plugins

Babel is a compiler(source code => output code). Like many other compilers it runs in 3 stages

* parsing
* transforming
* printing

解析 => 转换 => 打印输出



现在的Babel，需要添加插件后，才能做一些实际的工作，否则解析后会输出同样的代码。

除了一个个的添加插件，还能以`preset` 的形式启用一组插件。



### Transform Plugins 转换插件

进行代码转换, 会使对应的语法插件(syntax plugin)生效，因此不需要特别指定。



### Syntax Plugins 语法插件

allow Babel to parse specific types of syntax(not transform)

使Babel能够解析对应类型的语法（而不是转换）



### Plugin Ordering 插件顺序

```
Ordering matters for each visitor in the plugin
```

if two transforms both visit the "program" node, the transforms will run in either plugin or preset order.

* Plugins run before Preset, 插件在Presets前运行
* Plugin ordering is first to last.插件顺序从前往后排列
* Preset ordering is reversed ( last to first )，Preset顺序是颠倒的（从后往前）





```
{
  "presets": ["es2015", "react", "stage-2"]
}
依次执行：stage-2, react, es2015

为了确保向后兼容，因为大部分用户把es2015放在stage-0前面。
```



#### Plugin Options 插件参数

plugins和presets插件都能进行可选的配置, 参数由插件名和参数对象组成一个数组，可以在配置文件中设置。

plugins和presets的参数配置方式一样。

```
{
  "plugins": [
    [
      "transform-async-to-module-method",
      {
        "module": "bluebird",
        "method": "coroutine"
      }
    ]
  ]
}

module和method就是transform-async-to-module-method插件的参数配置
```





### Presets

preset就是一些预置的插件集合，避免用户一个个列出所有插件。



官方的presets:

* @babel/preset-env
* @babel/preset-flow
* @babel/preset-react
* @babel/preset-typescript

`Preset`是逆序排列的（从后往前）



### Caveats(注意事项)

不推荐使用`@babel/polyfill`，因为文件比较大，，，除非真的是那种低版本的IE，为了全部兼容，可以直接引入。
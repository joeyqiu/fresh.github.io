## Usage

### Config Files

Babel有两种配置文件格式，可以混合使用，或者单独使用：

* Project-wide configuration, 项目范围的配置
* File-relative configuration, 文件相关的配置
  * .babelrc文件
  * `package.json` 中的`babel` 选项



#### Project-wide configuration

Babel7中，有个`root` 目录的概念，会自动在该目录下找寻`babel.config.js/cjs/json` 文件。或者用户可以使用一个`configFile` 配置来覆盖默认的配置文件查找行为。

项目范围的配置，可以通过设置`configFile: false` 来禁止。



__api.cache__

In general, you should choose JSON over JS wherever possible

JS配置文件比较好的是可以配置，但是这就是的缓存比较困难。babel wants to avoid re-executing the config function every Time a file is compiled, because then it would also need to re-execute any plugin and preset functions referenced in that config.

```
api.cache.forever()
api.cache(true)

上述两个方法在不需要改变的js配置文件中，能够有json的效果，不需要重新编译运行
```





#### File-relative configuration

File-relative的配置可以通过设置`babelrc: false` 来禁止。



### babel-cli

cli命令的安全，更推荐局部安装

* 机器上不同的项目，可以使用不同版本的babel
* 局部安装可以使得项目更便于移植和安装



### @babel/plugin-transform-runtime

A plugin that enables the re-use of Babel's injected helper code to save on codesize.

使用Babel的注入机制，来使得代码更小。

```
Instance methods such as "foobar".includes("foo") will not work since that would require modification of existing built-ins
因为不修改内置的对象，所以一些内置的实例方法就不能使用了。
```



Babel在一些通用函数上使用一些很小的helpers（帮助函数？），如`_extend` 。默认情况下，这边helpers会被添加到每个有需要的文件中，这就会导致很多不必要的重复，尤其是你的应用通过按需加载分割成多个文件的时候。所以需要用`transform-runtime`插件来优化

目前看来，这个插件主要有两个功能:

* 所有的helpers都会引用模块`@babel/runtime`来避免重复，最终只有一份帮助函数
* 给代码创建一个沙盒环境，避免污染全局变量

如果使用的是`@babel/polyfill` ，则会修改内置的Promise等对象，污染全局环境。如果你的代码需要提供给第三方使用，污染全局变量是一个错误的方法。

transfromer会在使用内置对象的时候，自动引用`core-js` 中的对象，而不需要polyfill。



#### transform-runtime的技术细节

 其实就是做了三件事情：

* 在使用 generators/async函数的时候自动请求`@babel/runtime/regenerator` , (通过开关`regenerator`选项)
* helpers使用`core-js` 而不是通过polyfill垫片(通过开关`corejs` 选项)
* 自动删除内联的Babel helpers，转而使用`@babel/runtime/helpers` (通过开关`helpers` 选项)


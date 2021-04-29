## 模块[module]

在模块化编程中，开发者将程序分解为功能离散的chunk(discrete(分离的) chunks of functionality)，并称之为__模块__.

每个模块具有比完整程序更小的接触面，使得校验、调试、测试轻而易举。精心编写的模块提供了可靠的抽象和封装界限，使得应用程序中每个模块都具有调理清楚的设计和明确的目的。



### 什么是webpack模块？

与 Node.js 模块 相比，webpack _模块_能够以各种方式表达它们的依赖关系。下面是一些示例：

* ES2015 import 语句
* CommonJS require() 语句
* AMD define 和 require 语句
* css/sass/less 文件中的 @import 语句。
* 样式(`url(...)`)或 HTML 文件(`<img src=...>`)中的图片链接



除了上述内置支持的模块外，通过loader，webpack还能支持各种语言和预处理器语法编写的模块。

loader描述了webpack如何处理非javascript模块，并且在bundle中引入这些依赖。



## 配置

### Rule.oneOf

规则数组，当规则匹配时，只使用第一个匹配规则。



### Rule.use

应用于模块的UseEntries列表。每个入口指定使用一个loader。

Loaders can be chained by passing multiple laoders, which will be applied from right to left ( last to first configured )

loader列表的链接是从右往左执行，也就是从后面往前一个个loader执行


## 模块

这些选项决定了如何处理项目中不同类型的模块。



### strictExportPresence

makes missing exports an error instead of warning

如果有丢失的导出，会直接抛出错误而不是警告。



### noParse

防止webpack解析那些任何与给定正则表达式相匹配的文件。忽略的文件中不应该含有`import`, `require`, `define`的调用，或任何其它导入机制。忽略大型的library可以提高构建性能。



### rules

创建模块时，匹配请求的规则数组。这些规则能够修改模块的创建方式。这些规则能够对模块应用`loader`，或者修改解析器`parser`。



## Rule

每个规则可以分为三部分-条件、结果和嵌套规则



### Rule条件

两种输入值：

* resource: 请求文件的绝对路径。
* issuer：被请求资源的模块文件的绝对路径，是导入时的位置。

在规则中，属性 `test`, `include`, `exclude` 和 `resource` 对 resource 匹配，并且属性 `issuer` 对 issuer 匹配。



### Rule结果

规则结果是在规则条件匹配时使用。规则有两种输入值：

* 应用的loader: 应用在resource上的loader数组
* Parser选项：用于为模块创建解析器的选项对象

这些属性会影响 loader：`loader`, `options`, `use`。

也兼容这些属性：`query`, `loaders`。

`enforce` 属性会影响 loader 种类。不论是普通的，前置的，后置的 loader。

`parser` 属性会影响 parser 选项。



### 嵌套的Rule

可以使用属性的`rules`和`oneOf`指定嵌套规则

这些规则用于在规则条件匹配时进行取值。



### Rule.test

`Rule.test`是`Rule.resource.test`的简写。如果提供了一个`Rule.test`选项，就不能再提供`Rule.resource`



### Rule.parser

解析选项对象，所有应用的解析选项都将合并。

解析器可以查阅这些选项，并相应的禁用或重新配置。大多数默认插件，会如下解释值：

* 将选项设置为false，将禁用解析器
* 将选项设置为true，或不修改将其保留为`undefined`，可以启动解析器。



### Rule.enforce

可能的值有： `pre` | `post`

指定loader种类，没有值表示是普通的loader。



### Rule.use

可以是一个应用于模块的`UseEntries`数组。每个入口指定使用一个loader。



### Rule.oneOf

规则数组，当规则匹配时，只使用第一个匹配规则。






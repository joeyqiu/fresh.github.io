## CSS 模块化



### CSS模块化方案分类

目前自然css模块化的解决方案比较多，但主要是三类：

* 命名约定

规范化CSS的解决方案如：BEM、 OOCSS、AMCSS、SMACSS



* CSS in JS

彻底抛弃CSS，用javascript写CSS规则，`styled-components`就是其中代表



* 使用JS来管理样式模块

使用JS编译原生的CSS文件，使其具备模块化的能力，代表是CSS Modules



## 使用CSS Modules

CSS Modules不是将CSS改造的具有编程能力，而是加入了局部作用域、依赖管理，这恰恰解决了最大的痛点。可以有效避免全局污染和样式冲突，最大化地结合现有CSS生态和JS模块化能力。



A CSS Module is a CSS file in which all class names and animation names are scoped locally by defualt.



在react提供的demo中，使用的css-loader版本是2.1.1，目前css-loader的版本已经更新到了3.2.0，所以在配置方面有些不同，需要看下文档。

因为现在使用less的语法，所以用less module为例子

### css-loader: 2.1.1

```
{
	importLoaders: 2,
	sourceMap: isEnvProduction && shouldUseSourceMap,
	modules: true,
	getLocalIdent: getCSSModuleLocalIdent,
}
```

### css-loader: 3.1.0

```
{
	importLoaders: 2,
	sourceMap: isEnvProduction && shouldUseSourceMap,
	modules: true,
}
```

去掉getLocalIdent的配置，就行了



### modules的设置

如果设置为true，则都是用默认的最优配置，或者像自己坐下配置的，可以传对象属性

```
modules: {
	localIdentName: '[path][name]__[local]--[hash:base64:5]'
}
```



Recommendations:

- use `[path][name]__[local]` for development
- use `[hash:base64]` for production



[hash:base64]：默认返回长度为23的字符串




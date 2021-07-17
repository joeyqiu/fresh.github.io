预设是Babel插件的组合，就是可以避免自己去挑选插件，一些官方的预设如下：

```
@babel/preset-env
@babel/preset-flow
@babel/preset-react
@babel/preset-typescript
```

预设，就是插件列表集合。

Preset 是逆序排列的（从后往前）。

```
{
  "presets": [
    "a",
    "b",
    "c"
  ]
}
```

将按如下顺序执行： c、b 然后是 a。

这主要是为了确保向后兼容，由于大多数用户将 "es2015" 放在 "stage-0" 之前。

### @babel/preset-env

env是一个聪明的预设，允许你在各种目标环境中使用最新的JavaScript而不需要管理相关的语法转换（或者polyfill）。

env使用一个映射表：https://github.com/babel/babel/blob/master/packages/babel-compat-data/data/plugins.json 在任何环境中，检查映射表，然后得到满足条件的插件列表传给babel。

### @babel/preset-flow

Flow语法是fb公司开发的，用于react的语法。 使用Flow语法的时候推荐使用，是一个代码的静态类型检查器。包含了如下插件：

```
@babel/plugin-transform-flow-strip-types
```

### @babel/preset-react

总是包含如下插件：

```
@babel/plugin-syntax-jsx
@babel/plugin-transform-react-jsx
@babel/plugin-transform-react-display-name
```

### @babel/preset-typescript

使用TypeScript的时候使用的预设。包含如下插件：

```
@babel/plugin-transform-typescript
```

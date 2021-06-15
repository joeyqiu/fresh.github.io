## devtool

此选项控制是否生成，以及如何生成source map.



### devtool

选择一种source map格式来增强调试过程。不同的值会明显影响到构建(build)和重新构建(rebuild)的速度。

```
你可以直接使用 SourceMapDevToolPlugin/EvalSourceMapDevToolPlugin 来替代使用 devtool 选项，因为它有更多的选项。切勿同时使用 devtool 选项和 SourceMapDevToolPlugin/EvalSourceMapDevToolPlugin 插件。devtool 选项在内部添加过这些插件，所以你最终将应用两次插件。
```


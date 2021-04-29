## 优化

从webpack4开始，会根据你选择的`mode`来执行不行的优化，不过所有的优化还是可以手动配置和重写。



mode就是指production还是development，或者none。不同的mode，内置了一些不同的优化项。



### minimize

告知webpack使用`TerserPlugin`压缩bundle



### minimizer

允许你通过提供一个或多个定制过的TerserPlugin实例，覆盖默认压缩工具。



### splitChunks

对于动态导入模块，默认使用webpack v4+提供的全新的通用分块策略。可在SplitChunksPlugin页面中查看配置项。



### runtimeChunk

将`optimization.runtimeChunk`设置为`true`或者`multiple`，会为每个仅含有runtime的入口起点添加一个额外的chunk。






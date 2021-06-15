## SplitChunksPlugin

通常来说，模块（或者其中导入的模块）在webpack的关系图表中，是通过一种父-子的管理链接着。以前都是通过`CommonsChunkPlugin` 来避免这种重复的依赖，但是更深入的优化就不支持了。

 从webpack v4开始，`CommonsChunkPlugin`就删除了，并用`optimization.splitChunks` 取代。



### 默认

开箱即用的`SplitChunksPlugin` 对大部分用户来说都会很好用。

默认情况下，只影响按需加载的模块，因为修改初始模块，会影响html加载的script脚本。

webpack在以下情况下，会自动分割模块：

* 模块可以共享，或者是从`node_modules` 导入的模块
* 经过压缩或者gzip之后，大于30kb的模块
* 按需加载的的并行请求模块，最大数量应该<=5
* 在初始页面，并行请求的最大模块数量应该<=3

如果考虑后面两个需求的话，大一点的chunk比较合适。



### 配置

webpack提供一系列最优性能的默认配置，如果需要更多的控制，可以自己测量下影响，确保性能优化。



#### optimization.splitChunks

`SplitChunksPlugin` 插件的默认配置

```
module.exports = {
  //...
  optimization: {
    splitChunks: {
      chunks: 'async',
      minSize: 30000,
      maxSize: 0,
      minChunks: 1,
      maxAsyncRequests: 5,
      maxInitialRequests: 3,
      automaticNameDelimiter: '~',
      name: true,
      cacheGroups: {
        vendors: {
          test: /[\\/]node_modules[\\/]/,
          priority: -10
        },
        default: {
          minChunks: 2,
          priority: -20,
          reuseExistingChunk: true
        }
      }
    }
  }
};
```





#### splitChunks.automaticNameDelimiter

用来指定生成名字的分隔符



#### splitChunks.chunks

指定哪些chunks会被选择进行优化。可提供的字符串：

* all: 尤其有效，使用all就意味着chunk可以在异步和同步的chunks之间共享
* async 
* initial

或者使用函数来进行更细粒度的控制。返回值会声明是否包含



#### splitChunks.maxAsyncRequests

按需加载时候，最大的并行请求数



#### splitChunks.maxInitialRequests

入口七点的最大并行请求数量



#### splitChunks.minChunks

指定代码分割钱，在模块之间分享的chunk的最小数量。



#### splitChunks.minSize

chunk生成的最小单位？bytes



#### splitChunks.maxSize

告诉webpack，需要把大于maxSize尺寸的chunk分割成更小的部分。



#### splitChunks.name

指定分割模块的名字。如果提供的是布尔值`true`, 会基于chunks和缓存的key来自动生成一个名字。或使用字符串和一个函数来指定名字。

建议：设为false，在生产环境中不做额外的改动（不需要的改动）


















## Presets

`@babel/preset-env` is a smart preset that allows you to use the latest js without needing to micromanage which syntax transforms are need by your target environments.

提供通用的预置功能，是你能够使用最新的js功能，而不需要麻烦的去配置了，，只需要通过配置一个`target`字段，指定目标环境



A Babel preset is a shareable list of plugins.



#### targets

通过targets来指定项目支持的环境。

如果不设置target，没有指定目标环境，`@babel/preset-env` 会转换所有的es2015+ 代码，不推荐。

```
{
  "presets": [
    [
      "@babel/preset-env",
      {
        "targets": {
          "esmodules": true
        }
      }
    ]
  ]
}
```





从Babel v7开始，所有处于标准提案阶段的preset都已经废弃，也就是说如下的preset都已经不能用了:

* stage-0
* stage-1
* stage-2
* stage-3




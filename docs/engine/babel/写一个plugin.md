写一个简单的plugin，然后本地是用`yalc`工具进行软链接。

参考手册：https://github.com/jamiebuilds/babel-handbook/blob/master/translations/zh-Hans/plugin-handbook.md#toc-writing-your-first-babel-plugin

一个简单的把`==`改成`===`的插件。

比如命名为：babel-plugin-equal 源码：

```
export default function({types: t}) {
  return {
    visitor: {
      BinaryExpression(path, state) {
        if (path.node.operator === '==') {
          path.node.operator = '===';
        }
      }
    }
  };
};
```

项目中是用的时候，在`.babelrc`中配置下是用

```
{
    "plugins": ["babel-plugin-equal"]
}
```
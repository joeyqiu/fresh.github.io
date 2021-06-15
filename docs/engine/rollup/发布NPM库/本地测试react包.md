## 本地测试npm包

开发完react组件包之后，经常想着要本地验证下，如果是直接publish之后，升级到最新再去调试，如果有问题，会更新很多很多次的版本号，这就不好了。所以需要有个比较好的方案去本地验证。



### 软连接

通过`npm link` 命令，把本地包链接到全局的`node_modules` ，然后再测试项目中验证

### yalc工具

使用yalc工具来提供本地的仓库，然后项目中引入本地库来测试



这边准备用yalc工具来帮助验证本地的包，yalc的介绍：https://www.npmjs.com/package/yalc

yalc其实使用方式和npm/yarn类似，只是把仓库创建在了本地



yalc需要每次更新，但是软链接的行为更合适，不需要每次重新push。



这边需要两个项目：

- npm包：放置源码和编译后的代码
- react项目：用来验证npm包打包出来的代码



npm包中需要使用的命令

```
npm run dev 
yalc publish 或者 yalc push命令

推荐使用push命令，直接把包推送到react项目中去了，就不需要在react项目中执行update操作了
```

先编译下，打包出测试代码后，再把包推送到本地仓库中去



react项目中使用的命令

```
yalc add my-package 添加本地包  yalc update my-pack 更新本地包
```



问题：

目前试验下来，yalc update更新包之后，是react项目配置问题么，需要重新 start下。

如何解决，是因为react项目中起了`webpack-dev-server` 之后，在重新编译的时候，忽略了所有`node_modules` 中的项目，所以需要关注下`webpack-dev-server` 中的监听配置

```
watchOptions: {
    ignored: ignoredFiles(paths.appSrc)
},
```

如果使用的是`create-react-app` 的脚手架的话，推荐时直接在`node_modules/react-dev-utils/ignoredFiles.js` 中直接修改正则，忽略我们本地包的目录，这边本地包都是以`@zero` 开头，所以最终修改如下

```
module.exports = function ignoredFiles(appSrc) {
  return new RegExp(
    `^(?!${escape(
      path.normalize(appSrc + '/').replace(/[\\]+/g, '/')
    )}).+/node_modules/(?!@zero)`,
    'g'
  );
};
```

这样在npm包中push之后，react项目可以直接重新编译到新代码
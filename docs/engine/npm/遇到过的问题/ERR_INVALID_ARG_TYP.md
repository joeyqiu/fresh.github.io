## ERR_INVALID_ARG_TYP

```
TypeError [ERR_INVALID_ARG_TYPE]: The "path" argument must be of type string. Received type undefined
    at validateString (internal/validators.js:125:11)
    at Object.join (path.js:1147:7)
    at Object.<anonymous> (/Users/qiujinming/Documents/mine/github/Mine&/react-base/node_modules/@storybook/core/dist/server/manager/manager-webpack.config.js:169:29)
    at Module._compile (internal/modules/cjs/loader.js:701:30)
    at Object.Module._extensions..js (internal/modules/cjs/loader.js:712:10)
    at Module.load (internal/modules/cjs/loader.js:600:32)
    at tryModuleLoad (internal/modules/cjs/loader.js:539:12)
    at Function.Module._load (internal/modules/cjs/loader.js:531:3)
    at Module.require (internal/modules/cjs/loader.js:637:17)
    at require (internal/modules/cjs/helpers.js:22:18)
    at Object.<anonymous> (/Users/qiujinming/Documents/mine/github/Mine&/react-base/node_modules/@storybook/core/dist/server/manager/manager-preset.js:19:46)
    at Module._compile (internal/modules/cjs/loader.js:701:30)
    at Object.Module._extensions..js (internal/modules/cjs/loader.js:712:10)
    at Module.load (internal/modules/cjs/loader.js:600:32)
    at tryModuleLoad (internal/modules/cjs/loader.js:539:12)
    at Function.Module._load (internal/modules/cjs/loader.js:531:3)
```

遇到上述错误信息





### 场景

安装storybook 的时候，安装之后，总是跑不起来，总是报上面的错误



### 解决

查看错误信息, `manager-webpack.config.js:169:29` ，然后定位到这一行，发现是这段代码

```
recordsPath: _path.default.join(cacheDir, 'records.json'),
```

然后打印发现这边的 cacheDir是undefined。然后看cacheDir

```
var _findCacheDir = _interopRequireDefault(require("find-cache-dir"));

const cacheDir = (0, _findCacheDir.default)({
  name: 'storybook'
});
```

cacheDir是通过`find-cache-dir` 插件来寻找`node_modules` 下`.cache`目录的`storybook`目录，得到最终的路径，，但是在项目的`node_modules` 下没有这个`.cache/storybook` 目录，方案有两种：

* 注释掉recordsPath: 因为影响不大，可以注释
* 在node_modules下创建.cache/storybook目录



## 问题

这边的`.cache` 目录哪来的啊，，不是很懂，推测是storybook工具自动创建的，但是没成功么
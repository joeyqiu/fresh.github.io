在命令行上执行node命令的时候，可以通过console来进行打印，但是还是不如debugger模式来的方便。

如何使用debugger模式

使用inspect参数来启动debugger模式

```
node inspect myscript.js
```

然后在对应的js文件中添加debugger；

```
// myscript.js
global.x = 5;
setTimeout(() => {
  debugger;
  console.log('世界');
}, 1000);
console.log('你好');
```

但是这边遇到问题，通过`npm run build`命令执行的，如何添加inspect参数呢？

```
"build": "NODE_ENV=production webpack",

=>

"build-debug": "node inspect ./node_modules/webpack/bin/webpack.js",
```

目前只找到通过node命令的方式来启动debugger

### debugger的参数

退出debugger模式。

```
debug> .exit
```

### 单步执行

- cont, c: 继续执行。
- next, n: 单步执行下一行。
- step, s: 单步进入。
- out, o: 单步退出。
- pause: 暂停运行中的代码（类似于开发者工具中的暂停按钮）。

一般是使用n命令来进入下一行

### 信息

- backtrace, bt: 打印当前执行帧的回溯。
- list(5): 列出脚本源码的 5 行上下文（前后各 5 行）。
- watch(expr): 将表达式添加到监视列表。
- unwatch(expr): 从监视列表中移除表达式。
- watchers: 列出所有的监视器和它们的值（在每个断点上自动地列出）。
- repl: 打开调试器的 repl，用于调试脚本的上下文中的执行。
- exec expr: 在调试脚本的上下文中执行一个表达式。

一般进入debug模式后，如果在通过n命令一行行调试的时候，想要查看当前上下文的变量名，则输入`repl` 命令，可以进入上下文查询

```
break in node_modules/webpack-cli/bin/utils/convert-argv.js:79
 77 	} else {
 78 		const defaultConfigFileNames = ["webpack.config", "webpackfile"].join("|");
>79 		const webpackConfigFileRegExp = `(${defaultConfigFileNames})(${extensions.join("|")})`;
 80 		const pathToWebpackConfig = findup(webpackConfigFileRegExp);
 81 
debug> repl
Press Ctrl + C to leave debug repl
> defaultConfigFileNames
'webpack.config|webpackfile'
```

如果觉得控制台展示的代码量太少了，可以通过`list(n)`命令来设置展示前后多少行。
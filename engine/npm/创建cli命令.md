##  创建自定义的cli命令

在学习react的过程中，就发现`create-react-app` 这个全局的命令，其实我们也可以自己写一个全局的命令。



### 本地的全局cli命令

在项目的`package.json` 中写一个`bin`的配置，指向的脚本需要按照文档说明的来，在这个脚本的顶部写上``#!/usr/bin/env node``，用来声明是node脚本。

如：

```
{
"name": "my-lib",
"version": "1.0.0",
"bin": {
	"my-lib": "./index.js"
  }
}
```

这边的`index.js` 中一些node相关的脚本代码。项目搞定后，通过在项目中执行npm link`命令，把项目连接到全局目录中去，并把bin命令软连接到全局的bin中去。

这样一个本地环境的全局cli就安装好了。主要靠的就是`npm link` 命令



### 配置npm上的cli

如果把本地的cli项目，上传到npm包上去，然后再通过`npm install -g xxx` 的方式来安装，就是一个线上的全局cli命令了。。  

最重要的就是`bin` 的配置。


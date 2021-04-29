## Working with package.json

A `package.json` file:

* lists the packages that your project depends on.(列出项目以来的包)
* Allows you to specify the versions of a package that your project can use using semantic versioning rules.（通过语义版本规则来控制依赖包的版本）
* Makes your build reproducible, and therefore much easier to share with other developer.(更方便和别的开发者分享)

完整的文档：https://docs.npmjs.com/files/package.json



字段介绍

* [bin](#bin)
* [files](#files)
* [main](#main)





## 版本号

### Semver for Publishers

If a project is going to be shared with others, it should start at `1.0.0`, after this, changes should be handled as follows:

| Code status                                                  | Stage         | Rule                                                         | Example version |
| ------------------------------------------------------------ | ------------- | ------------------------------------------------------------ | --------------- |
| First release                                                | New product   | Start with 1.0.0                                             | 1.0.0           |
| Backward compatible bug fix(向后兼容的bug修复)               | Patch release | Increment the third digit                                    | 1.0.1           |
| Backward compatible new feature(向后兼容的新功能)            | Minor release | Increment the middle digit and reset last digit to zero      | 1.1.0           |
| Changes that break backward compatibility(修改向后兼容的重大改动) | Major release | Increment the first digit and reset middle and last digits to zero | 2.0.0           |



### Semver for Consumers

As a consumer, you can specify which kinds of updates your app can accept in the `package.json` file.

If you were starting with a package 1.0.4, this is how you would specify the ranges:

- Patch releases(补丁修改): `1.0` or `1.0.x` or `~1.0.4`
- Minor releases(小版本): `1` or `1.x` or `^1.0.4`
- Major releases: `*` or `x`



###  X 范围

字符 `X`、`x` 或者 `*` 都可以作为通配符，用于填充部分或全部版本号。



### ～ 字符范围

同时使用字符 `~` 和次版本号，表明允许`修订号`变更。同时使用字符 `~` 和主版本号，表明允许`次版本号`变更。



### ^ 字符范围

字符 ^ 表明不会修改版本号中的第一个非零数字，`3.1.4` 里的 `3` 或者 `0.4.2` 里的 `4`。



## package.json字段



### bin

A lot of packages have one or more executable files that they'd like to install into the PATH. npm makes this pretty easy.

supply a `bin` field in your package.json which a map of command name to local file name.  On install, npm will symlink(符号链接) that file into `prefix/bin` for global installs, or `./node_modules/.bin/` for local installs.

npm在执行指定script之前会把`node_modules/.bin`加到环境变量`$PATH`的前面，意味着任何内含可执行文件的npm依赖都可以在 npm script 中直接调用。

比如

```
{ "bin" : { "myapp" : "./cli.js" } }
```

当你安装的时候，会创建一个软连接，从cli.js脚本到/usr/local/bin/myapp。如果这边的包名就是myapp，这边可以简化成一个字符串。

```
{
"name": "myapp",
"bin": "./cli.js"
}
```

和下面的相等。

```
{"name": "myapp", "bin":{"myapp": "./cli.js"}}
```

这边有个重点，就是请确保bin指向的文件，需要在最顶部添加`#!/usr/bin/env node`， 否则脚本不会用node的方式来执行。



### files

the optional __files__ field is an array of file patterns that describes the entries to be included when your package is installed as a dependency



### main

the main field is a module ID that is the primary entry point to your program. That is, if your package is named __foo__, and a user installs it, and then does __require('foo')__ , then your main module's exports object will be returned.

this should be a module ID relative to the root of your package folder.

main指定了一个模块ID，是程序的主入口




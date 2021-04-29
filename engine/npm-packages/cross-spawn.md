## cross-spawn

文档地址：https://www.npmjs.com/package/cross-spawn

A cross platform solution to node's spawn and spawnSync.

`child_process.spawn()` 方法使用给定的 `command` 和 `args` 中的命令行参数来衍生一个新进程。 如果省略 `args`，则默认为一个空数组。



### Why?

Node has issues when using spawn on windows:

* it ignores PATHEXT
* it does not support shebangs
* has problems running commands with space
* Has problems running commands with posix relative path
* has an issue with command shims, where arguments with quotes and parenthesis would result in invalid syntax error
* No options.shell support on node < v4.8




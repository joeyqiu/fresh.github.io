## npm link

symlink  a package folder

符号链接一个包的目录



### 使用

```
npm link (in package dir)
npm link [<@scope>/]<pkg>[@<version>]

alias: npm ln
```



### 说明

包链接是一个有着两种步骤的进程。

* npm link

在包的目录中执行`npm link` 命令，会在全局的`{prefix}/lib/node_modules/<package>` 目录下创建一个软连接，连接到执行命令的当前目录。

而且任务包里面的bins配置，都会连接到`{prefix}/bin/{name}`去。prefix可以通过`npm prefix -g` 来查看值。

* npm link package-name

然后在别的目录中，执行`npm link package-name` 会从全局安装的`package-name` 中，创建一个软连接到当前目录的`node_modules/` 中来。

`package-name`都是通过`package.json`中的`name`字段来设置的。



The package name can be optionally prefixed with a scope. See [`scope`](https://docs.npmjs.com/using-npm/npm-scope). The scope must be preceded by an @-symbol and followed by a slash.

包还能够设置范围，范围需要在包名前用@scope/name的方式。
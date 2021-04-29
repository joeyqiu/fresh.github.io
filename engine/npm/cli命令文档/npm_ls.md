## npm ls

文档地址： https://docs.npmjs.com/cli-commands/ls.html

列出项目中安装的所有包，和各自的依赖（应该是只看dependencies了，devDependencies之类的是不列出来的）

```
npm ls [[<@scope>/]<pkg> ...]

aliases: list, la, ll
```

This command will print to stdout all the versions of packages that are installed, as well as their dependencies, in a tree-structure.

这个命令会在控制台输出安装的所有包，以及依赖包，用树形结构的形式。



### configuration

Display only the dependency tree for packages in `dependencies`.

只看生产依赖的包，比较推荐这个吧

```
npm ls --production
```







### 输出信息

在输出信息中经常可以看到deduped的信息，这是包的重复，可以通过`npm dedupe `来清理部分重复

```
│ ├─┬ mkdirp@0.5.4
│ │ └── minimist@1.2.5 deduped
```

也会输出extraneous的信息，表示安装了不需要的软件包，可以清理掉，实际上只是一个警告，因此可以忽略。可以通过`npm prune` 命令处理无关的包

```
├── core-util-is@1.0.2 extraneous
```


## npm dedupe

文档地址： https://docs.npmjs.com/cli-commands/dedupe.html

```
npm dedupe

别名： find-dupes, ddp
```

减少重复



Searches the local package tree and attempts to simplify the overall structure by moving dependencies further up the tree, where they can be more effectively shared by multiple dependent packages.

会查询本地包的树形结构，然后通过移动依赖来优化整体的结构。是的包能够更有效的在多个依赖中分享

Note that this operation transforms the dependency tree, but will never result in new modules being installed.

该操作只会移动依赖树，但是并不会导致安装新的包，所以是无影响的。
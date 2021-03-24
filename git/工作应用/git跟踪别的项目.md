## git跟踪别的项目

场景如：学习ant-design，从github上clone了一份代码，然后自己可能删删减减的修改下，再push到自己的仓库中去。ant-design的更新又比较勤快，这个时候怎么跟踪ant-design的更新？



方法：

在项目中保留两个远程分支，一个是自己仓库的，一个是github上仓库的。一般如果是自己仓库的话，remote会自动有origin分支指向自己的远程仓库，然后通过`git remote` 命令来添加另外一个仓库。

```
git remote add <name> <url>

如
git remote add official https://github.com/ant-design/ant-design.git
// 一般默认的remote是origin，这个official是官方的远程
```



然后基于这个官方的远程仓库创建新的分支：

```
git checkout -b latest official/master，基于官方的远程master分支，创建个本地的
```

latest分支是跟踪github上ant最新代码的分支。

如果需要merge新功能的时候，可以基于latest再切换一个新分支，然后会推到想要merge的版本，然后再merge到自己的分支上去。

latest分支只做跟踪。
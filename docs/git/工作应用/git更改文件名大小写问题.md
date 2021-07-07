#### 场景：

有个application的目录，但是在代码中的写的是`Application`，大写的格式，代码运行的时候没有问题，但是在服务器上打包的时候，会报找不到该文件目录的错误提示。

所以修改目录名字，但是执行`git status .`的时候发现没有任何的改动。



#### 原因与解决方案

Git配置默认忽略了大小写，需要配置为不忽略大小写

查看是否忽略大小写：

```shell
git config core.ignorecase
```

> 备注：true为忽略了大小写，false为不忽略大小写



设置为不忽略大小写

```shell
git config core.ignorecase false
```

就可以把改动提交上去了。
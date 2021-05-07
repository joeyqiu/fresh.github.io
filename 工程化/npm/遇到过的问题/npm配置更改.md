## npm配置更改



有时候一些npm的配置被缓存的，然后想要修改的时候一直无法顺利更新，，，可以通过`npm config get`命令查看配置，一般都会有如下提示。

```
; userconfig /Users/xxxx/.npmrc
```

就其实电脑本地会保存一份`.npmrc`的配置文件，只需要删除该`.npmrc` 配置文件，然后就可以去操作了。s
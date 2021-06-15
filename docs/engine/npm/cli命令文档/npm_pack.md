## npm-pack

Create a tarball from a package

tarball是linux下最方便的打包工具，，因为可以理解为基于package创建一个压缩文件。



### 概要

```
npm pack [[<@scope>/]<pkg>...] [--dry-run]
```



### 详情

For anything that's installable(that is, a package folder, tarball, tarball url, name@tag, name@version, name, or scoped name), this command will fetch it to the cache, and then copy the tarball to the current working directory as <name>-<version>.tgz, and then write the filenames out to stdout.




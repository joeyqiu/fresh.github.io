## fs-extra

文档地址：https://www.npmjs.com/package/fs-extra

方法文档地址：https://github.com/jprichardson/node-fs-extra/tree/a32c85282185aa008759890cce059594e4348262/docs



adds file system methods that aren't included in the native `fs` module and adds promise support to the `fs` methods. It also uses `graceful-fs` to prevent `EMFILE` errors. It should be a drop in replacement for `fs`.(用来取代fs)



## why?

got tired of including `mkdirp`, `rimraf` and `ncp` in most of my projects. (牛逼作者就是任性)





### ensureDirSync(dir[,options])

ensures that the directory exists. 确保目录的存在, if the directory structure does not exists, it is created. Like `mkdir -p` . if provided, options may specify the disired mode for the directory.



### emptyDirSync(dir)

Ensures that a directory is empty.确保目录是空的, Deletes directory contents if the directory is not empty. if the directory does not exist, it is created. The directory itself is not deleted.

如果存在，清空

如果不存在，创建



### copySync(src, dest, [options])

Copy a file or directory. the directory can have contents. Like `cp -r`

* src: if src is a directory it will copy everything inside of this directory, not the entire directory itself.
* dest: if src is a file, dest cannot be a directory.
## node-gyp rebuild错误



npm install的时候遇到如下错误，其实包都是安装成功了，但就是爆出没有权限的错误信息，不知道什么原因。

按照别人的说法是即使用了sudo，还是自动切换成了没有权限的用户。

```

> fsevents@1.2.12 install /Users/qiujinming/Documents/work/JDWork/zero/jdyfe-tpls/tpl-bubble/node_modules/chokidar/node_modules/fsevents
> node-gyp rebuild

gyp ERR! configure error 
gyp ERR! stack Error: EACCES: permission denied, mkdir '/Users/qiujinming/Documents/work/JDWork/zero/jdyfe-tpls/tpl-bubble/node_modules/chokidar/node_modules/fsevents/build'
gyp ERR! System Darwin 18.7.0
gyp ERR! command "/usr/local/bin/node" "/usr/local/lib/node_modules/npm/node_modules/node-gyp/bin/node-gyp.js" "rebuild"
gyp ERR! cwd /Users/qiujinming/Documents/work/JDWork/zero/jdyfe-tpls/tpl-bubble/node_modules/chokidar/node_modules/fsevents
gyp ERR! node -v v10.15.3
gyp ERR! node-gyp -v v3.8.0
gyp ERR! not ok 
npm WARN @zero/jdyfe-tpl-bubble@0.1.1 No repository field.
npm WARN optional SKIPPING OPTIONAL DEPENDENCY: fsevents@1.2.12 (node_modules/chokidar/node_modules/fsevents):
npm WARN optional SKIPPING OPTIONAL DEPENDENCY: fsevents@1.2.12 install: `node-gyp rebuild`
npm WARN optional SKIPPING OPTIONAL DEPENDENCY: Exit status 1

added 1101 packages from 559 contributors in 12.839s
```



### 解决

```
 sudo npm install --unsafe-perm
```

官网文档： https://docs.npmjs.com/misc/config#unsafe-perm



### 说明

具体原因不明，，按照说法是，用了这个参数，加上sudo就是root用户，不会自动切换成没有权限的用户了。
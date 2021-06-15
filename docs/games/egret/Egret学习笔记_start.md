## Egret学习笔记


egret版本更新换代比较快，一些常用的操作等有时候也会没有效果，尽量在官网上找，而不是在百度上乱搜


### 自动编译的方式打开游戏
````
egret startserver -a
````

### build相关
如果更新了egretProperties.json文件，需要重新编一下，修改后新增的库才能够生效

````
egret build -e
````

如果每次都报出
`{ Error: EACCES: permission denied, open '/Users/qiujinming/Documents/study/egretGame/HelloWorld/libs/modules/egret/egret.d.ts'`，
可以换个终端窗口，clean一下再start


### publish相关
publish会打包在`bin-release`目录下面，然后就发现修改的index.html没有任何改变，这是因为采用的`template/web`下面的index.html文件。

也就是说，，debug的时候，采用的是目录下的index.html，然后最终打包的时候，采用的是template目录下的index.html文件。
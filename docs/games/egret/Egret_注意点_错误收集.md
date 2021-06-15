## 错误集合



### permission denied

如果每次都报出
`{ Error: EACCES: permission denied, open '/Users/qiujinming/Documents/study/egretGame/HelloWorld/libs/modules/egret/egret.d.ts'`，
可以换个终端窗口，clean一下再start





### 第三方库的 tsconfig.json 中必须包含 outFile 这一属性

肯定是因为通过node引入了`package.json`导致的问题，

把这些脚本移到项目根目录下一个单独的子目录（重新创建个目录叫build）里面，同时package.json文件也移动过去，然后重新执行npm install安装依赖，把根目录下原来的package.json和node_modules文件夹给删了就完美解决 ...
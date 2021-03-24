## build.js

`yarn build`脚本打包`create-react-app`生成的项目。



和`start.js`中的步骤差不多，不过`start.js`是启动服务，这边是打包编译。也就是直接调用`webpack(config);`方法编译。



检查浏览器，并检查`build`目录下的文件，并清空`build`文件夹。把`public`目录下的内容都拷贝到`build`目录下去。

通过build方法，来启动webpack的打包编译。



通过`compile.run(xxx)`方法来监听编译中的信息，比如编译过程中的err或者warning信息。



通过`printFileSizesAfterBuild`方法来打印编译后的文件大小，比如：

```
File sizes after gzip:

  38.5 KB  build/static/js/2.4f057907.chunk.js
  762 B    build/static/js/runtime~main.a8a9905a.js
  601 B    build/static/js/main.838a2cfb.chunk.js
  517 B    build/static/css/main.2cce8147.chunk.css
```








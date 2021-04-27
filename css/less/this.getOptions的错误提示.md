## this.getOptions is not a functon

#### 背景

通过creact-react-app命令创建的react项目模版，然后默认使用的是scss预编译，然后我还是习惯用less，所以切换到了less编译。

```
const lessRegex = /\.(less)$/;
const lessModuleRegex = /\.module\.(less)$/;
```

这个替换是比较方便的，搜索webpack.config.js中的scss，拷贝一份换成less就行了。

#### 现象

`yarn start` 执行之后，终端报如下的错误信息: 

```
./src/pages/home/index.less (./node_modules/css-loader/dist/cjs.js??ref--5-oneOf-7-1!./node_modules/postcss-loader/src??postcss!./node_modules/resolve-url-loader??ref--5-oneOf-7-3!./node_modules/less-loader/dist/cjs.js??ref--5-oneOf-7-4!./src/pages/home/index.less)
TypeError: this.getOptions is not a function
```

`home/index.less`就是我想引入的 less文件。

#### 分析

按照网上的说法，就是less-loader现在使用了较高的版本，，我看了下，现在安装的都是`^8.1.1`的版本。 也有是webpack版本的问题，cra默认安装的还是webpack4的版本。



#### 解决

可以有两种解决方法

* webpack4下，less-loader版本过高，所以降低版本到`7.3.0` 可以解决问题。
* 升级webpack版本到5，，这样应该也可能解决。（没有验证，不过想着loader出问题肯定就是为了兼容最新版本的webpack）


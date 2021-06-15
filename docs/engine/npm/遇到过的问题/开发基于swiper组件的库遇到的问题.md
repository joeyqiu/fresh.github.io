## 基于swiper组件开发使用的库，遇到的一些问题

### 库的说明

package.json中的配置，虽然是基于swiper的，但是没有把swiper放在dependencies中，没有强制安装，怕和引用方安装的有冲突。

库打包的时候，也是把react、react-dom、swiper、prop-types等代码排除在外了

```
{
...
"dependencies": {
    "prop-types": "^15.7.2"
  },
  "peerDependencies": {
    "react": "^16.13.1",
    "react-dom": "^16.13.1",
    "swiper": "^5.3.6"
  },
}
```

swiper引用的方式如下：

```
import Swiper from 'swiper'
import 'swiper/css/swiper.min.css';
```



最终实验下来，还是建议把swiper放在dependencies中去

```
{
"dependencies": {
    "prop-types": "^15.7.2",
    "swiper": "^5.3.6"
  },
  "peerDependencies": {
    "react": "^16.13.1",
    "react-dom": "^16.13.1"
  },
}
```



### 场景1

项目中，没有主动安装swiper的时候，会爆出swiper缺失的错误提示。

```
./node_modules/@zero/jdyfe-tpl-bannerswiper/dist/index.esm.js
Module not found: Can't resolve 'swiper' in '/Users/qiujinming/Documents/work/JDWork/zero/react-tpl/node_modules/@zero/jdyfe-tpl-bannerswiper/dist'
```



### 场景2

组件peerDependencies中设置的swiper组件版本是5.3.6，然后项目安装了5.3.0版本的swiper。

然后发现一切OK，没有问题的顺利运行着，应该是5.3.6和5.3.0两个版本之间，没有用到两者不同的API，所以可以顺利使用。



### 场景3

使用更低版本的swiper组件，比如5.0.0，然后发现报错，hhh，应该就是swiper依赖的版本问题。

![](https://img13.360buyimg.com/imagetools/jfs/t1/130848/21/2032/1426435/5ee22fbbEcdeac07b/672278a062e4f5da.png)



### 场景4

想在组件中依赖swiper，然后安装组件的时候自动安装，再在项目中安装不同版本的swiper，看下是否打包的时候会把两个版本的代码合并在一起?

* 组件中依赖的是5.3.6版本的swiper，然后项目中直接用`npm install swiper` 命令安装，没有指定版本，结果最新版本的swiper把组件安装的5.3.6版本覆盖了，所以最终只有一个版本的swiper。
* 项目中先安装了5.4.2版本的swiper，然后再安装组件，发现组件中5.3.6版本的swiper没有安装成功，应该是使用了5.4.2版本的swiper
* 先安装组件，会自动安装默认的5.3.6版本的swiper，然后在项目中指定5.4.2版本的swiper安装，看会怎么样？居然把swiper的版本升上去了

测试下来，难道是因为用的是公司源的npm包么，怎么突然这么智能了。



### 总结

测试下来，建议在组件中，把swiper放在dependencies中。



### 低版本webpack下，uglifyjs打包遇到的错误

低版本的webpack，一般都使用`UglifyJsPlugin` 插件来进行代码的压缩，然后插件在遇到没有经过babel转义的代码会报错。

```
Failed to minify the bundle. Error: static/js/2.db44d78f.chunk.js from UglifyJs
Unexpected token: operator (>) [static/js/2.db44d78f.chunk.js:79,43]
```

等各种由于Uglify插件导致的错误。

问题产生的原因就是，swiper组件和依赖的dom7，都在node_modules目录下，然后配置的时候，默认不会对node_modules目录下的代码进行babel转义（默认都是已经转义过的）。

所以解决方法其实也简单，就是在js的模块加载中，把node_modules下，这两个库加到解析的的路径下就行。include中去。

```
test: /\.(js|jsx|mjs)$/,
            include: [
              resolve('node_modules/_swiper@5.4.2@swiper/'),
              resolve('node_modules/_dom7\@2.1.5\@dom7/'),
              paths.appSrc
            ],
```





### 不同版本的swiper

#### 5.3.0的包结构：

![](/Users/qiujinming/Documents/work/doc/前端工具/npm/遇到过的问题/imgs/swiper_yarn.png)

#### 5.0.0

![](https://img10.360buyimg.com/imagetools/jfs/t1/145617/5/464/14611/5ee22d4cEf78028e6/4a19fcb1ec699293.png)

#### 4.5.1

![](https://img14.360buyimg.com/imagetools/jfs/t1/135214/15/2089/63746/5ee22dc6E0ca52600/d4c192c66b269631.png)



### 不同版本webpack下，swiper引入问题的深入分析

就版本的webpack是3.8.1，新版本的swiper是4.40.2。其实都是bannerSwiper组件来依赖到swiper的

### V3版本的webpack

* 引入的是module入口的bannerSwiper.js代码
* bannerSwiper引入的swiper，引入的也是module入口的swiper.esm.bundle.js代码

swiper.esm.bundle.js代码是没有转码过的



### v4版本的webpack

* 引入的也是module入口的bannerSwiper.js代码
* bannerSwiper引入的swiper，入口居然是main的swiper.js代码

swiper.js代码是不用转码的基础代码，所以在新版本的webpack中，不会遇到转义的问题
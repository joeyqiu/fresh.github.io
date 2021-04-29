## vendor.js

package.json中的依赖

```
@babel/core
@babel/runtime-corejs3
@jdyfe/jdyfe-jump
@jdmm/jmdd-to
@jmfe/jm-jdreminder
@jmfe/jm-jdshare
@jmfe/jm-wxuserinfo
axios
better-scroll
core-js
prop-types
react
react-dom
react-fastclick
react-lazyload
react-router
react-router-dom
redux-devtools-extension
svgaplayerweb
swiper
```







代码中用到

```
react
react-dom
react-fastclick
react-redux
redux
redux-devtools-extension
redux-thunk
immutable
react-router-dom


vconsole
```



文件一行行看下来的话，vendor.js中，应该就是使用到的库文件都集成在了一起。

然后剩下的main.js文件中，也把大量的公共代码都放在了这边，，，多了很多core-js的代码，可能是babel引入的？


# Guides

React Router 中有三种类型的组件：

* router components : HashRouter/BrowserRouter
* route matching component: Route
* navigation components: Link

上述三个组件都需要从`react-router-dom`中引入。

```
import {BrowserRouter, Route, Link} from 'react-router-dom';
```



## Routers

the core of React Router application should be a router component.

提供了`<BrowserRouter>`和`<HashRouter>`两种，这两种都会创建一个专用的history对象。一般来说：

* BrowserRouter: 如果你有一个服务需要响应请求，最好用浏览理由
* HashRouter: 如果仅仅是一个静态文件服务



## Route Matching

有两个路由匹配组件：`<Route>` and `<Switch>`

路由匹配的执行是通过比较`<Route>`的path值和当前location的`pathname`值。如果匹配则渲染，否则返回null。

如果没有置顶Route的path，则一直渲染。



Swtich组件用来包裹在Route外面，不是必须的。Switch会遍历包裹的所有Route组件，然后仅仅渲染第一个匹配的Route内容。



## Navigation

`<Link, <NavLink>, <Redirect>`三个组件，一般用Link。

Link会创建一个`<a>`标签锚在html中。



## Code Splitting

代码分割，为了实现这个功能，需要使用`webpack, @babel/plugin-syntax-dynamic-import, react-loadable`.



## Scroll Restoration

滚动恢复



## philosophy（原理）

try to explain the mental modal to have when using React Router. We call it 'Dynamic Routing', which is quite different from the 'Static Routing' you're probably more familiar with.

### static Routing

you declare your routes as part of your app's initialization before any rendering takes place.

在app的初始化时定义好路由。



### Dynamic Routing

Routing takes place as your app is rendering, not in a configuration or convention outside of a running app.

That means almost everything is a component in React Router.



## Redux Integration








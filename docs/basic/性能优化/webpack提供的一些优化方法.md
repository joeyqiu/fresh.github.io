## webpack提供的一些优化方法

为了优化加载时间，webpack提供的的优化方法有：

* 防止重复
* 代码分离
* 按需加载/懒加载
* 缓存
* tree shaking来清理没有引用的代码



## 防止重复

通过SplitChunksPlugin插件来把共享并且改动频率很小的文件抽离出来那种



## 代码分离

通过模块中的内联函数调用来分离代码。

```
import A from './A'
```

这种方式算是静态导入，会把这个A模块的代码合并到父模块中去



```
import(/* webpackChunkName: "lodash" */ 'lodash').then(({ default: _ }) => {
	xxxx
}).catch(error => 'An error occurred while loading the component');

import(/* webpackChunkName: "Modal" */ './modal').then(({ default: Modal }) => {
	ReactDOM.render(<Modal options={options}  />, document.body);
}).catch(error => 'An error occurred while loading the component');
```

这种算是动态导入，会把模块的代码单独抽离出来，然后再需要的时候再载入。



## 按需加载

就是上面的动态导入，在需要的时候加载，一般就是在某个行为触发后，再通过`import().then()`动态载入。



按需加载其实就是上面的代码分离意思，通过import算是静态引入，打包的时候会把所有代码都打包在一起。但是其实有些库的代码不是必须的，或者说是首屏必须的，所以可以通过动态的`import()`方法，导入之后再去渲染。





## 缓存

打包的时候，只有代码改动的部分才会修改文件名，去重新引入。

库文件等不会修改的就固定不变，可以直接使用浏览器缓存。



## Tree Shaking


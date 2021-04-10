## 弹窗后面的滚动如何禁止

在一个上下可以滚动的页面上，有一个居中的弹窗，弹窗也有两种情况：

* 内部可以滚动
* 内部不能滚动（不能滚动的比较简单）

现在先不考虑弹窗内部。然后展示了弹窗之后，在屏幕上上下滑动的时候，滑动事件会传递到body上，然后导致弹窗后面的页面也跟着滚动了。现在有一个需求就是在弹窗出现后，禁止页面的滚动。



### 相关的的代码

弹窗的html部分代码：

```html
<body>
	xxx body比较长，能够上下滚动
	<div class="panel">
		<div class="mask">弹窗的黑色背景，一般都有透明度</div>
		<div class="inner">弹窗内部的内容</div>
	</div>
</body>
```

弹窗的样式代码：

```css
.panel {
	position: fixed;
	width: 100%;
	height: 100%;
	top: 0;
	left: 0;
	z-index: 10002;
}
.panel .mask {
	position: absolute;
	width: 100%;
	height: 100%;
	top: 0;
	left: 0;
}
.panel .inner {
	position: absolute;
	width: 400px;
	height: 500px;
	top: 50%;
	left: 50%;
	transform: translate3d(-50%,-50%,0);
	background-color: #F5F5F5;
	border-radius: 16px 16px 0 0;
}
```

弹窗会罩住当前页面，然后mask的背景也是整个的宽高。



### 普通实现

如果是在普通项目中（非react项目），只需要在panel上加个监听

````
function addStopParentScroll(selector){
	document.querySelector(selector).addEventListener('touchmove', stopScroll, false)
}
function removeStopParentScroll(selector){
	document.querySelector(selector).removeEventListener('touchmove', stopScroll, false)
}
function stopScroll(e) {
	e.preventDefault(); //防止默认行为，这样就不会触发scroll事件了
}

在panel上添加：
addStopParentScroll('.panel')
````



### 在React项目中的实现

在react项目中，想当然的是直接在标签上添加onTouchMove事件，然后也是调用preventDefault来防止滚动。但是react中的事件监听确实passive形式的，所以preventDefault是无效的。只能如上面那样来添加。

````
componentDidMount(){
	addStopParentScroll('.panel')
}
componentWillUnmount(){
	removeStopParentScroll('.panel')
}
````



### passive的事件

passive事件监听器是一个新的浏览器特性，通过passive来告诉浏览器，当前页面内注册的事件监听器内部是否会调用preventDefault函数来阻止事件的默认行为，一般浏览器更好的做出决策来优化页面性能。

当passive为true的时候，表示内部不会调用preventDefault（调用了也是无效的）函数来阻止默认滑动行为，主要用该特性来优化页面的滑动性能，所以当前的Passive Event Listener特性仅支持mousewheel/touch相关的事件。

当passive为false的时候，因为内部需要做判断是否滚动，所以导致流畅性在判断的时候会降低。



### 内部需要滚动的弹窗

如果是内部需要滚动的弹窗，请参考stone项目中的scorll部分，有现成的插件可以使用。
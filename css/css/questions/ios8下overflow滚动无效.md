## ios8下overflow滚动无效

ios8下，一个弹出层

```html
<div class="record-panel hide">
	<div class="mask" />
	<div class="card">
		xxx
		<div class="inner"></div>
	</div>
</div>

inner这边有比较过的内容，固定高度后，需要可以滚动
```

```
.record-panel {
	position: fixed;
	width: 100%;
	height: 100%;
	top: 0;
	left: 0;
	z-index: 999999;
}
.mask {
	position: absolute;
		width: 100%;
		height: 100%;
		top: 0;
		left: 0;
}
.panel-card {
		z-index: 999999;
		position: absolute;
		width: 100%;
		bottom: 0;
		left: 0;
		background-color: #F5F5F5;
		border-radius: 16px 16px 0 0;
		&.show-panel {
			display: block;
			animation: showPanelVer .3s 1 ease-in;
	   	animation-fill-mode: forwards;
		}

		&.hide-panel {
			display: block;
			animation: hidePanelVer .3s 1 ease-in;
	   	animation-fill-mode: forwards;
		}
	}
	
.inner {
		position: relative;
		height: 886px;
		z-index: 99;
		overflow-y: auto;
		-webkit-overflow-scrolling: touch;
	}
```

上述代码在高版本的手机上都没有问题，但就是在ios8下面，会有滚动性的问题，卡住了就是没有滚动。

就像手机的焦点都在最外面的面板上，inner就是无法滚动。

这边收集了一些解决方案，可能需要组合一起使用，但是完全不知道原理是啥。



## z-index

当你给一个元素设置过`position:absolute;`或者`position:relative;`后再增加`-webkit-overflow-scrolling: touch;`属性后，你会发现，滑动几次后可滚动区域会卡主，不能在滑动，这时给元素增加个z-index值就可以了。

```
.inner {
		position: relative;
		height: 886px;
		z-index: 1;
		overflow-y: auto;
		-webkit-overflow-scrolling: touch;
	}
```



## animation-fill-mode: forwards;

这边的面板上，默认是隐藏的，在展示的时候用了一个animation动画来展示。

最终发现是`animation-fill-mode: forwards;`属性导致卡住，不能滚动，，

* 或者不用animation来展示，直接display：block展示
* 或者是把animation-fill-mode属性给注释掉

上述两种方法都能解决当前遇到的问题，但是都比较神奇


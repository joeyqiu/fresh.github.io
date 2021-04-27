## 300ms延迟

移动端点击事件的300ms延迟。一般情况下，如果没有经过特殊处理，移动端浏览器在派发点击事件的时候，通常会出现300ms左右的延迟。



### Reason

移动端浏览器上支持__双击缩放__操作，以及ios safari上的双击滚动操作，是导致300ms的点击延迟主要原因。

假定这么一个场景。用户在 iOS Safari 里边点击了一个链接。由于用户可以进行双击缩放或者单击跳转的操作，当用户一次点击屏幕之后，浏览器并不能立刻判断用户是确实要打开这个链接，还是想要进行双击操作。因此，iOS Safari 就__等待 300 毫秒__，以判断用户是否再次点击了屏幕。



300ms的延迟，是否是一个问题，主要依靠网页的使用场景，如果不在意这300ms的延迟，就不需要处理。

### 解决方案

* Disable Zooming(Chrome and Firefox)

if zooming has been disabled using the following viewport setting in your HTML Head;

以前是chrome和firefox支持，在safari上有问题，但是现在safari正在完善中。

```
<meta name="viewport" content="width=device-width, user-scalable=no">

or

<meta name="viewport" content="width=device-width, initial-scale=1, minimum-scale=1, maximum-scale=1">
```



* Set the viewport to the device-width(Chrome32+)

Chrome32, 设置视口宽度<= device-width，就能禁止缩放，就能达到上面的效果。

```
<meta name="viewport" content="width=device-width, initial-scale=1, minimum-scale=1, maximum-scale=3">
```



* Use PointerEvents(IE10+)

```
a, button, .myelements
{
	-ms-touch-action: manipulation;	/* IE10  */
	touch-action: manipulation;		/* IE11+ */
}
```



* Implement touched Event Handlers

Unlike `click` and `touchstart`, `touchend` events are fired instantly without a 300ms delay. touchend事件不会有延迟。





1. A User firing `touchstart` on a different element to `touchend` should not fire the click.
2. A User firing `touchstart` then a reasonably large `touchmove` before the `touchend` event is scrolling -- a click should not be fired.
3. You still require standard `click` handlers for those using non-touch devices -- but be way about firing the same event twice.





## 场景复现

```
<button id="click">Click Me!</button>

document.getElementById('click').addEventListener('touchend', function(e) {
  time1 = Date.now();
},false);

document.getElementById('click').addEventListener('click', function(e) {
  alert(Date.now() - time1);
},false);
```

通过监听click事件和touchend事件，来获取其中的间隔时间。touchend事件是会立即触发，而不带300ms延迟的。



不带所有`<meta>`标签的时候，上面的代码间隔时间，大概率都是在300ms左右，少数会卡在比较大的数值。

如果带上`<meta>` 标签，代码间隔时间，大概率的就是个位数的毫秒时间，，，少数会卡在比较大的数值。



## 原理

在touchend事件中发送click事件。




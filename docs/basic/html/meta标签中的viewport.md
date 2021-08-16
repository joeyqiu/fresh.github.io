## meta: viewport

viewport: 为viewport(视口)的初始大小提供指示(hint)，目前仅用于移动设备。

参考链接：https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/meta/name

| 值    | 可能的附加值                          | 描述                                                 |
| ----- | ------------------------------------- | ---------------------------------------------------- |
| width | 一个正整数，或者字符串 `device-width` | 定义 viewport 的宽度，如果值为正整数，则单位为像素。 |



### 禁止页面缩放

在html标签中通过设置meta，可以禁止页面的缩放。

```html
<meta name="viewport" content="width=device-width, initial-scale=1, user-scalable=0">
```



### shrink-to-fit=no

下面的一行代码可以让网页的宽度自动适应手机屏幕的宽度:

```
<meta name="viewport" content="width=device-width, initial-scale=1">
```

 但在iOS9中要想起作用，得加上`"shrink-to-fit=no"`，原因如下:
 *Viewport meta tags using "width=device-width" cause the page to scale down to fit content that overflows the viewport bounds. You can override this behavior by adding "shrink-to-fit=no" to your meta tag as shown below. The added value will prevent the page from scaling to fit the viewport.*



### viewport-fit

iphoneX的“刘海”为相机和其他组件留出了空间，同时在底部也留有可操作区域。那么网站边尴尬了~被限制在了这样的“安全区域”内，两边会出现一道白条。解决的方案是：1、给body添加一个background；2、添加viewport-fit=cover meta标签，使页面占满整个屏幕。

```
<meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover">
```






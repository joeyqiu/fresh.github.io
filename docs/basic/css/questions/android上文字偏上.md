## android上文字偏上

参考网址：

```html
https://rprns.me/2018/07/27/关于%20Android%20下%20line-height%20文字垂直居中偏移的思考/?nsukey=viim2gyL1g3OEY2gxUeXs4hs7WGH93U7OKGjAWQiIAdE7QP%2FkOidiVlRx5ipQ1jeM6HO60j8mOyEAltHX%2BS2B1NZBWPAL30qUGqrfJ0%2Be0BIm0o1OwjbQUkOY6v9VFb4968GXeRtX0fQHTEykSQWm33W%2FDFjNzeAAysOnZfv9LeISi%2B5xRxwXWZs88UUdppzPNXFBgpMUTj0g5HHdDlNfQ%3D%3D
```



主要原因还是设置了line-height导致的，android手机上渲染时查找文字会出错，具体解决方法，在低版本的手机上不是很好处理.



### 高版本的手机上

```css
.text-center {
  line-height: normal;
  display: inline-flex;
  align-items: center;
  justify-content: center;
}
```

重置`line-height` 属性，然后用flex居中。在高版本的Android手机上看着是可以，低版本的不确定。



### 解决方法

搞清楚字体偏移的原因后，需要解决这个问题。

1. 一个方案就是上面知乎大佬给出的方案。`font-family` 避开 `fonts.xml` 里设置了 alias 的那些字体并设置 `html` 的 `lang` 属性。Android 7.0 以下中文字体的英文丑的问题，需要和设计师一起评估。
2. 除了这个方案，另一个就是，在需要垂直居中的场合，不设置 `line-height`，保持其默认值 `normal`，而完全用 `padding` 将容器撑起来。这个方案的缺点在于，不同字体的 `line-height: normal` 的值不尽相同，会导致容器实际高度与设计稿有出入。毕竟移动端 web 设计稿基本都是以 iOS 为基准设计的，而 Android 机型鱼龙混杂，涉及字体的视觉还原本身难以进行。实测发现，iOS 的 Pingfang 与 Android 7.0+ 的 Noto Sans CJK 字体在默认 `line-height` 值下每个 `font-size` 所对应实际文字高度相差不超过 `1px`，个人认为是完全可以接受的，也可以与设计师一起评估。
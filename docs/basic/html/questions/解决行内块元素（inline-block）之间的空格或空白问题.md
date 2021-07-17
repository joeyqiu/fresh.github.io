## 解决行内块元素（inline-block）之间的空格或空白问题

一行两个div元素，都是`inline-block`，空间也是基本够的，但还是被挤压下去了，导致两个div变成了一个一行。

```
<div class="star3_card">
</div>
<div class="star3_card">
</div>
```

原因就是两个div之前，有空白区间的存在，导致元素被挤压。



可以有如下解决方法：

* 不换行

```
<div class="star3_card">
</div><div class="star3_card">
</div>
```

可以解决，但是不美观

* **给父元素设置font-size：0**

缺点：子元素如果需要字体的话，会需要重新在子元素添加fon-size的设置。但如果像我一样是图片不需要文字的话，就很完美了。

* **不用inline-block改为float**

float是忽略空白符的，不过你的CSS布局可能要重新花一下心思，可能会涉及到清除浮动之类设置。

* **white-space-collapse**

这个属性还不支持，就比较麻烦，用来设置或者检索元素内包含的空白字符，有如下取值：

- collapse：将一系列空白折叠为一个单独的字符（或者在某些情况下，没有字符）
- preserve：阻止用户代理折叠空白，换行符保留为强制换行符。
- preserve-breaks：该值将与「collapse」一样折叠空白字符，但保留换行符为强制换行符。
- discard：抛弃所有空白。
  现在该属性被转移到《CSS Text Level 4》中，该规范中， 「white-space」分为两部分：white-space-collapse和text-wrap


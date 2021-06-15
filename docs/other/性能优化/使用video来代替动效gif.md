## 使用video来代替动效gif

如果gif特别特别大(动不动就几M，甚至10+M)，然后又不会显得特别重要的话，，可以通过video来实现动效的替换。



### 把gif转换成视频格式

There are a number of ways you can convert GIFs to video, but [`ffmpeg`](https://www.ffmpeg.org/) is the tool we'll use in this guide. 

一般建议是转换成mp4和WebM两种格式，mp4是支持最广泛的，但是webM的格式性能好一些，然后让浏览器去自己选择。



### 用video标签取代img标签

gif有三种特点：

1. 自动播放
2. 持续循环播放
3. 静默

所以video标签可以设置如下：

```
<video autoplay loop muted playsinline></video>

muted属性用来自动播放，然后playsinline在ios平台上需要用
```



video标签从上往下，不会自动选择，所以需要把webM的格式放上面。

```
<video autoplay loop muted playsinline>
  <source src="oneDoesNotSimply.webm" type="video/webm">
  <source src="oneDoesNotSimply.mp4" type="video/mp4">
</video>
```



### video vs gif的性能

性能方面，webM的视频最棒，然后是mp4，最后才是大尺寸的gif图片（前提是大尺寸）。



### 潜在的缺陷



#### video不如gif方便

gif都是图片的集合，所以在各种平台下的支持更好，而且处理video比较麻烦，可能转换的时候需要很多时间.



#### Data saver mode

在android上，自动播放是很大概率受限制的。比如Data Saver开启的时候（完全不懂，应该是app原生的设计），如果无法自动播放，可以试下关闭Data Saver.



#### 一些无法自动播放的兜底方案

* 如果自动播放受限，然后一定要用户点击之后才能自动播放的话，其实可以在video上加一个模糊的涂层，等用户点击之后，再播放video，并把涂层隐藏掉。给用户一种加载的感觉
* 如果页面中只有一个video，或者通过弹窗交互的方式进行触发










浏览器处理动画顺序 ![顺序](https://img-blog.csdnimg.cn/20181205113632374.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L01pbmdsZUhEVQ==,size_16,color_FFFFFF,t_70&ynotemdtimestamp=1626536332381)

1. 获取dom：获得我们所有的dom树
2. 分割图层：浏览器根据z-index，和脱离了原来dom层的dom解构进行分层。
3. 计算样式：分解图层完毕后，将所有的图层批量进行样式计算。这里有些属性是CPU去计算的，有些属性是GPU去计算的，具体哪些，请看下文。
4. reflow-relayout-paint set up-repaint:这一系列过程其实是页面从回流到重绘发生的步骤，这也是为什么回流必然引起重绘，而重绘却不一定要回流的原因。
5. GPU：重绘后的“画布”交给GPU去处理。
6. 组合布局：浏览器组合布局，然后页面就出现了。

### 说明

同一时间内至少存在一个页面初始化layout行为和一个绘制行为（除非你的页面是空白页-blank）。在此之后，改变任何影响构造渲染树的行为都会触发以下一种或者多种动作：

1. 渲染树的部分或者全部将需要重新构造并且渲染节点的大小需要重新计算。这个过程叫做回流-reflow，或者relayout（这词是原文作者杜撰的，为了标题中多个“R”）。浏览器中至少存在一个reflow行为-即页面的初始化layout；
2. 屏幕的部分区域需要进行更新，要么是因为节点的几何结构改变，要么是因为格式改变，如背景色的变化。屏幕的更新行为称作重绘-repaint，或者redraw。 重绘和回流的性能消耗是非常严重的，破坏用户体验，造成UI卡顿。

但是相比较而言，回流是比重绘更严重些。

### 触发重排relayout的属性

1）盒子模型相关属性：

- width * height * padding * margin * display * border-width * border * min-height（width） * max-height(width) 等等。

2）定位和浮动

- top * bottom * left * right * position * float * clear

3）改变节点内部文字结构

- text-align * overflow-y * font-weight * overflow * font-family * line-height * vertical-align * white-space * font-size

### 触发重绘的属性

- color * border-style * border-radius * visibility * text-decoration * background * background-image * background-position * background-repeat * background-size * outline-color * outline * outline-style * outline-width * box-shadow

### GPU加速 硬件加速

硬件加速是指将属性交给GPU处理，根本原因是创建了新的layer，属性改变直接由GPU处理，加快处理速度。使得有一些属性的改变可以略过relayout，减少浏览器在动画运行时所需要做的工作。

最常用的是3d转换（translate3d）。

缺点：GPU使用会增加内存消耗，同时也会影响字体的抗锯齿效果，导致文本在动画期间会显得有些模糊

### 什么情况下，浏览器会创建新的layer，开启GPU加速？

在webkit内核的浏览器中，如果有上述情况，则会创建一个独立的layer：

1）transform（3d转换）

2） video标签

3）混合插件(如 Flash)

1. isolation == isolate

5）opacity < 1

6）filter != normal

7）z-index != auto || 0 + 父元素display: flex|inline-flex

1. mix-blend-mode != normal

9）position == fixed || absolute

10)-webkit-overflow-scrolling == touch

11)will-change:指定可以形成新layer的元素。

其实，在css3中，最推荐的还是使用transform:translateZ(0);来创建一个新的layer，成本低，影响小。

除了GPU加速，还有一个比较好用的属性，可以告诉浏览器什么属性将会发生变化，让浏览器做好最佳处理:

will-change：存在兼容性问题。

### transform3D和2D

首先transform和绝对定位都会产生新的图层，所以都不存在重排，图层在GPU中transform又不会引起重绘，这就是硬件加速的原理。另外，transform3D和2D的区别在于3D渲染前便会产生新的图层，而2D是在运行时产生图层，运行结束时删除图层。
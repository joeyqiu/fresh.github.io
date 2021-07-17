一般都是More Tools => Animations 打开展示动画面板

The Animation Inspector supports CSS animations, CSS transitions, and web animations. requestAnimationFrame animations are currently not supported. 动画面板目前能检查到CSS动画，CSStransition，和web动画，但是不支持requestAnimationFrame的动画。

web动画：https://developer.mozilla.org/zh-CN/docs/Web/API/Web_Animations_API/Using_the_Web_Animations_API 将交互式动画从样式表移动到javascript，将表现与行为分开。

支持情况：https://caniuse.com/?search=Animation

### 用途

chrome开发者的动画面板，主要有以下两个目的：

- 检测动画(Inspecting animations): 可以检测动画组的减速、重播效果，或者查看源码
- 修改动画(Modifying animations): You want to modify the timing, delay, duration, or keyframe offsets of an animation group. Bezier editing and keyframe editing are currently not supported.

### what's an animation group?

An animation group is a group of animations that appear to be related to each other. 动画组是一组有相互关系的动画

### 面板组成

![组成](https://img14.360buyimg.com/imagetools/jfs/t1/126547/33/12691/193539/5f6094b7E65f28a7c/a34743b0a15146c5.png?ynotemdtimestamp=1626536001502)

从上往下依次是四个部分组成。

- Controls(控制): From here you can clear all currently captured Animation Groups, or change the speed of the currently selected Animation Group.
- Overview(总览): Select an Animation Group here to inspect and modify it in the Details pane.
- Timeline(时间轴): Pause and start an animation from here, or jump to a specific point in the animation.
- Details(详情): Inspect and modify the currently selected Animation Group.

### 修改动画

在动画检测面板，可以修改一个动画的下面三个部分：

- Animation duration 动画时间
- keyframes timing 帧时间
- Start time delay 开始前的延迟时间

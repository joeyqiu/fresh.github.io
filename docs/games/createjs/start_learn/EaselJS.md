## EaselJS





## 坑

* spritesheet的framerate目前试下来没有效果，不知道为什么，只能通过指定的animation去实现

```
var spriteSheet = new createjs.SpriteSheet({
	"images": [this.loader.getResult('car')],
	"framerate": 5,
	"frames": [
		[1, 1, 142, 233, 0, 0, 0],
		[145, 1, 141, 230, 0, 0, 0],
			[288, 1, 147, 215, 0, 0, 0]
	]
});
var animation = new createjs.Sprite(spriteSheet);
animation.play();
```

理想的效果是1秒5帧的动画效果，但是实际运行下来，没有效果，动画太快了。只能通过指定animation，然后这边的speed是有效果的

```
var spriteSheet = new createjs.SpriteSheet({
	"images": [this.loader.getResult('car')],
	"framerate": 5,
	"frames": [
		[1, 1, 142, 233, 0, 0, 0],
		[145, 1, 141, 230, 0, 0, 0],
			[288, 1, 147, 215, 0, 0, 0]
	],
	"animations": {
		"run": [0,2, 'run', 0.2]
	}
});
var animation = new createjs.Sprite(spriteSheet, "run");
```

这边指定的"run"中，第四个值表示speed，能够生效。





* 背景图片拉伸之后如何剧中对齐，下面用一个竖屏的例子

```
var bgScene = new createjs.Container;
var bgItem = new createjs.Bitmap(this.loader.getResult('bg'));
var bgBounds = bgItem.getBounds();
// 竖屏，所以高度撑满，宽度自适应
var scaleValue = this.stageHeight/bgBounds.height;
bgItem.scaleX = scaleValue;
bgItem.scaleY = scaleValue;
bgScene.addChild(bgItem);

this.stage.addChild(bgScene);
```

上述情况下，如果需要在x轴居中对齐的话，我原本想着用下面的方法就好了 ：

```
bgItem.regX = (bgBounds.width * scaleValue - this.stageWidth)/2;
//错误代码，结果试下来，总是没有居中对齐，比较偏右了，不知道为什么
```

后来直接在bgScene上做居中处理，就好了，所以正确的操作应该是在外层的Container上做居中对齐操作

```
bgScene.x = -((bgBounds.width * scaleValue - this.stageWidth)/2);
// or
bgScene.regX = ((bgBounds.width * scaleValue - this.stageWidth)/2);
```

上述两种方式都可以.

结论：

scale等操作，只能在Bitmap那个元素上，就是你要操作的元素上，而不是容器上





* scaleX和scaleY等动画居中变化

```
const circleImage = new createjs.Bitmap(this.loader.getResult('intro_circle'));
const circleBounds = circleImage.getBounds();
circleImage.alpha = 0.6;
circleImage.x= this.stageWidth/2 - circleBounds.width/2;
circleImage.y= this.stageHeight - circleBounds.height;
```

circleImage居中展示，这时候如果修改scaleX和scaleY来进行放大缩小，中心点是左上角，所以左上角固定的方式，向左向下进行缩小放大。

```
circleImage.regX = circleBounds.width/2;
circleImage.regY = circleBounds.height/2;
```

通过设置regX和regY来修改中心点，上面把中心点设置在图片的中心位置。但是设置过regX和regY之后，之前设置的x和y的位置会有变化，需要重新设置一下，，所以可以先设置中心点，然后再设置x和y的距离。最终修改如何下

```
const circleImage = new createjs.Bitmap(this.loader.getResult('intro_circle'));
const circleBounds = circleImage.getBounds();
circleImage.alpha = 0.6;
circleImage.regX = circleBounds.width/2;
circleImage.regY = circleBounds.height/2;
circleImage.x= this.stageWidth/2;
circleImage.y= this.stageHeight - circleBounds.height/2;
```


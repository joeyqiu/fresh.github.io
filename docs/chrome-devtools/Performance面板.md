性能记录面板: 可以记录运行时性能信息，也可以记录页面加载时候的信息。

![按钮](https://img12.360buyimg.com/imagetools/jfs/t1/138626/31/8667/59901/5f6457b9Efc89773d/6be860f061990eba.png?ynotemdtimestamp=1626535829278)

- Record runtime performance: 点那个record按钮可以开始一个新的记录，需要手动stop
- Record load performance： 点那个reload按钮可以开始记录页面的加载状态，会自动stop。

### 界面

![性能面板](https://img14.360buyimg.com/imagetools/jfs/t1/123594/17/12652/424893/5f61b309E45c95f63/a13de07d8f350bb3.png?ynotemdtimestamp=1626535829278)

文档写着当你记录页面性能前，记得点一下垃圾回收的按钮。然后勾选左边的`screenshots`勾选框，这个可以在记录每一帧的时候获取一个截屏。 ![垃圾回收](https://img10.360buyimg.com/imagetools/jfs/t1/136040/12/10305/64435/5f6459b3E82f0c2c6/1876a9cb61df5294.png?ynotemdtimestamp=1626535829278)

#### settings

点击最右边的`settings`按钮，可以做一些记录方面的设置。

![settings](https://img10.360buyimg.com/imagetools/jfs/t1/122360/39/13023/52183/5f645a20Efaa7966a/2e82071f35dab444.png?ynotemdtimestamp=1626535829278)

#### Disable JavaScript samples

默认情况下，Main部分会显示详细的JS函数调用堆栈信息，如果不想显示的话，就可以勾选这边的`Diable JavaScript samples`选项，然后发现勾选之后的Main部分会简短很多。

![没有勾选的情况下](https://img10.360buyimg.com/imagetools/jfs/t1/154531/18/215/359366/5f645d1aE2e3a1782/3a828f4f38b587d9.png?ynotemdtimestamp=1626535829278)![勾选后](https://img14.360buyimg.com/imagetools/jfs/t1/138076/38/8657/420678/5f645d18E02f616ac/0a728ac61095dbfa.png?ynotemdtimestamp=1626535829278)

#### throttle

可以对网络和CPU性能进行节流，主要是模拟性能和网络比较差的情况。但是也只是模拟，和真实的情况还是有区别的。

![network throttle](https://img10.360buyimg.com/imagetools/jfs/t1/131265/5/10309/189406/5f645e66Eca774cf9/ca068d0d83363407.png?ynotemdtimestamp=1626535829278)![cpu throttle](https://img12.360buyimg.com/imagetools/jfs/t1/130217/1/10388/127523/5f645e66Eb7a5af9e/160e53fa81eb74cc.png?ynotemdtimestamp=1626535829278)

#### Enable advanced paint instrumentation

启动更加高级的绘制面板，这边选项开始后，可以在绘制时看到更多的信息，这个在下面会讲到。

### 分析

#### 分析每秒帧数

动画性能的主要测量单位就是每秒帧数(frames per second: FPS)。1秒60帧就会有比较棒的动画效果。

![FPS](https://img10.360buyimg.com/imagetools/jfs/t1/147571/31/8507/31286/5f61b46cEdec17f60/d41aa6cdf3179125.png?ynotemdtimestamp=1626535829278)首先先看FPS部分，这边图表上方的红条，就表示帧速率降的很厉害，可能就影响到了用户体验。一般是绿条越高，就表示每秒帧数就越多。

![CPU](https://img13.360buyimg.com/imagetools/jfs/t1/124499/17/12733/346549/5f61b5bdEfc808491/70623dedc9d11349.png?ynotemdtimestamp=1626535829278)然后我们在看FPS下面的CPU图表，这边图表中的颜色和下面Summary面板的颜色是一一对应的。Idle是白色的，所以这边CPU图表满满都是颜色，就说明在记录期间运行CPU的占用率很高。也就说明可以考虑优化了。

![screenshot](https://img14.360buyimg.com/imagetools/jfs/t1/152052/8/24/443120/5f61b709E808cd9c2/b3d1b1606f06e7ab.png?ynotemdtimestamp=1626535829278)CPU下面是NET部分，勾选了"screenshots"之后，这边会展示截屏，然后鼠标在FPS,CPU,NET这几个部分左右移动的时候，可以一帧帧的复现页面上的动画效果。

![Frames](https://img14.360buyimg.com/imagetools/jfs/t1/148480/31/8549/394648/5f61b973E31521011/a6440f52b9561660.png?ynotemdtimestamp=1626535829278)在Frames部分，这边有一个个的绿色小方块，表示的是每一个帧，当鼠标点击小方块上的时候，会展示当前帧的信息（看底下的Summary部分），这边是(50.0ms ~ 20fps)，每秒20帧，每帧消耗50ms，远低于60fps的理想值。

#### 打开FPS的仪表盘

![fps meter](https://img13.360buyimg.com/imagetools/jfs/t1/147247/21/8570/266575/5f61bb8fE873a35f0/0c43465dfa1054eb.png?ynotemdtimestamp=1626535829278)想更方便的看FPS相关的信息，可以在rendering面板勾选FPS meter.

### 找到瓶颈

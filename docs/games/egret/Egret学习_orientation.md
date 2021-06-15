# 环境变量之陀螺仪

参考地址：http://developer.egret.com/cn/github/egret-docs/Engine2D/multimedia/environment/index.html



在移动设备中，一般支持获取设备本身的旋转角度。`egret.DeviceOrientation` 可以监听设备方向的变化。



egret中可以用如下方式获取各种角度

```javascript
class DeviceOrientationExample extends egret.DisplayObjectContainer {
    private label: egret.TextField;
    public constructor() {
        super();
        this.label = new egret.TextField();
        this.label.y = 50;
        this.label.x = 50;
        this.addChild(this.label);
        //创建 DeviceOrientation 类
        var orientation = new egret.DeviceOrientation();
        //添加事件监听器
        orientation.addEventListener(egret.Event.CHANGE,this.onOrientation,this);
        //开始监听设备方向变化
        orientation.start();
    }
    private onOrientation(e:egret.OrientationEvent){
        this.label.text =
            "方向: nalpha:"+e.alpha
            +",nbeta:"+e.beta
            +",ngamma:"+e.gamma;
    }
}
```

`OrientationEvent`的三个属性：`alpha`,`beta`和`gamma`。

- `alpha`表示设备绕 Z 轴的角度，单位是 角度 范围是 0 到 360。
- `beta` 表示设备绕 X 轴的角度，单位是 角度 范围是 -180 到 180.这个值表示设备从前向后的旋转状态。
- `gamma` 表示设备绕 Y 轴的角度，单位是 角度 范围是 -90 到 90.这个值表示设备从左到右的旋转状态。



__onOrientation事件会被调用的特别频繁，因为手机的位置在不停的变动，可以通过throttle设定一个频率（在不需要这么频繁判断的时候）__
## 碰撞检测


参考地址：http://developer.egret.com/cn/github/egret-docs/Engine2D/hit/rectangleHit/index.html

egret的碰撞检测，目前有矩形碰撞检测和像素碰撞检测。

### 矩形碰撞检测

#### 判断对象是否与一点相交
```
var isHit:boolean = shp.hitTestPoint( x: number, y:number );
```

#### intersects

使用`egret.Rectangle`的intersects方法来判断是否相交

两个矩形之间检测，目前flybird的游戏的话，这种方式会简单很多

````
public intersects( toIntersect:egret.Rectangle ):boolean
````


### 像素碰撞检测

```
传递第三个参数即可
var isHit:boolean = shp.hitTestPoint( x: number, y:number, true:boolean );
```

__大量使用像素碰撞检测，会消耗更多的性能__
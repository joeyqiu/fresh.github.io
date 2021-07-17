## CSS 帧动画

参考饿了么的loading，需要把多张图片合成在一起，然后只能在一个方向合成，要么是一行，要么是一列。



```html
<style type="text/css">
.ele-loading {
  display: inline-block;
  width: 200px;
  height: 200px;
  transform: scale(.5);
  background-repeat: no-repeat;
  background-image:  url('https://img30.360buyimg.com/pop/jfs/t1/102663/25/16272/57232/5e7afabeE0b6f57e1/4cb23224f050fbaa.png');
  animation: loading 4s normal infinite steps(43);
}

@keyframes loading {
    0% {
      background-position-x: 0;
    }
    100% {
      background-position-x: 100%;
    }
}
</style>

<i class="ele-loading" />
```

原图的宽高是8800*200，也就是一共有44张图片合成在一张长图上。要求用正方形的图片，这样好计算，然后动起来的话，相当于移动了43步，所以animation这边的step用43.





#### 小鸟动画

```css
.loading-bird {
  display: inline-block;
  width: 330px;
  height: 330px;
  transform: scale(.5);
  background-repeat: no-repeat;
  background-image: url('https://img30.360buyimg.com/pop/jfs/t1/48154/10/5912/268801/5d3954d6E1928d702/6d2b9e26fbc9a59c.png');
  animation: pinkactive 4s normal infinite steps(18);
}

@keyframes pinkactive {
    0% {
      background-position-x: 0;
    }
    25% {
      background-position-x: 0;
    }
    100% {
      background-position-x: 100%;
    }
}
```

这边25%也是0，表示动画在开始的时候会保持一段时间才开始。做一个延迟的用途。
# AntDesign Colors

Ant Design 将色彩体系解读成两个层面：系统级色彩体系和产品级色彩体系。

系统级色彩体系主要定义了蚂蚁中台设计中的基础色板、中性色板和数据可视化色板。产品级色彩体系则是在具体设计过程中，基于系统色彩进一步定义符合产品调性以及功能诉求的颜色。

```
https://www.npmjs.com/package/tinycolor2
tinycolor2是一个js操作颜色的工具
```



## 色彩模式

* HSB色彩模式： HSB色彩模式即色度(Hue)、饱和度(Saturation)、亮度模式(brightness)。它采用颜色的三属性来表色，即将颜色三属性进行量化，饱和度和亮度以百分比值（0%-100%）表示，色度以角度（0°-360°）表示 。 HSB色彩模式以人类对颜色的感觉为基础，描述了颜色的三种基本特性 。

  色相（色度）就是纯色，即组成可见光谱的单色，红色在0，绿色在120，蓝色在240.

  饱和度代表颜色的纯度，为0时即为灰色。白、黑和其他灰度色彩都没有饱和度，最大饱和度时是每一个色相最纯的色光。

  亮度是指颜色的明亮度，为0时即为黑色，最大的亮度是色彩最鲜明的状态。

  

* HSL色彩模式：是HSB色彩模式的扩展，由色度(Hue)、光度(Lightness)和饱和度(saturation)这3个要素所组成的。色度决定颜色的面貌特质，光度决定颜色光线的强弱度，饱和度表示颜色纯度的高低。

  

* HSV颜色模型：HSV（Hue, Saturation, Value），分别是色调、饱和度、明度。明度表示颜色明亮的程度，对于光源色，明度值与发光体的光亮度有关；对于物体色，此值和物体的透射比或反射比有关，取值范围为0%（黑） 到 100%（白）。

  

* RGB模式：也称为光源色模式，原因是RGB能够产生和太阳光一样的颜色。R(red)、G(green)、B(blue)，通过RGB三种颜色的混合，可以生成自然界里任何一种颜色。



### HSV

antd里面的颜色值，是用的HSV色彩模式，在hsv三种值上进行的修改

* H： 色度不同，色度转向也不同,  0-360的整数
* S：从上到下，饱和度是从低到高，越上面的颜色，饱和度越低。0-100%的百分比，或者0-1的数字.
* V：从上到下，明度是从高到度，越上面的颜色明度越高。0-100%的百分比，或者0-1的数字.

颜色值的切换，使用到了`tinycolor2`插件，把普通颜色值转换成hsv的格式。

```
// color palettes
@drip-base: #f35180;
@drip-1: color(~`colorPalette('@{drip-6}', 1) `);
@drip-2: color(~`colorPalette('@{drip-6}', 2) `);
@drip-3: color(~`colorPalette('@{drip-6}', 3) `);
@drip-4: color(~`colorPalette('@{drip-6}', 4) `);
@drip-5: color(~`colorPalette('@{drip-6}', 5) `);
@drip-6: @drip-base;
@drip-7: color(~`colorPalette('@{drip-6}', 7) `);
@drip-8: color(~`colorPalette('@{drip-6}', 8) `);
@drip-9: color(~`colorPalette('@{drip-6}', 9) `);
@drip-10: color(~`colorPalette('@{drip-6}', 10) `);
```

color函数是less自带的，接受一个指定颜色的字符串。

就很奇怪，我在less文档中没有看到这种~``的用法和@functions自定义函数的方法，可能还需要看下文档。

但是从代码中可以看到，是在colorPalette.less中自定义了`colorPalette` 函数。基于每个基础色调，都会生成10个颜色值，第6个就是基础色调。



colorPalette具体的实现函数，js实现放在了`@ant-design/colors`包里面，

```
this.colorPalette = function(color, index) {
    var isLight = index <= 6;
    var hsv = tinycolor(color).toHsv();
    var i = isLight ? lightColorCount + 1 - index : index - lightColorCount - 1;
    return tinycolor({
      h: getHue(hsv, i, isLight),
      s: getSaturation(hsv, i, isLight),
      v: getValue(hsv, i, isLight),
    }).toHexString();
  };
```

颜色从1-10，第6个是基色，前面5个都是浅色，后面4个是深色。






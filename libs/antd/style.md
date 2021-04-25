

## AntDesign - style

antd中，每个组件的style里面都有个`index.tsx`文件，通过js的方式来引入样式代码。

所以在使用antd的时候，需要写引入样式部分的代码：

```
@import '~antd/dist/antd.css';
```

或者通过`babel-plugin-import`插件来实现自动引入样式代码。



### 组件中

组件中的style/index.tsx文件，基本都是如下，引入公用style目录和组件的index.less。

```
import '../../style/index.less';
import './index.less';
```

少部分组件会依赖其余组件，所以也需要把组件的样式也引入，比如slider组件中，就依赖了tooltip组件，所以slider的style中引入了tooltip的样式。

```
import '../../style/index.less';
import './index.less';

// style dependencies
import '../../tooltip/style';
```



### style

```
|--color -- *
|--core -- *
|--mixins -- *
|--themes -- *
|--compact.less
|--dark.less
|--index.less
|--index.tsx
```

index.tsx文件中引入了index.less.

```
# index.tsx
import './index.less';

# index.less
@import './themes/index';
@import './core/index';

# themes/index.less
@import './default.less';
```

themes目录中除了index.less外，另外三个文件都分别表示一种主题，default是默认的，dark.less和compact.less分别是antd支持的另外两个主题：暗黑模式、紧凑模式。

主题文件中，会定义很多通用的变量。



### default

具体还是要看代码，这边只展示部分

```
# color中会基于给定的基准颜色值，提供10种渐变的颜色。
@import '../color/colors';
```

Colors，主颜色的渐变，基本脚手架的变量。

```
@primary-color: @blue-6;
@info-color: @primary-color;
@success-color: @green-6;
@processing-color: @blue-6;
@error-color: @red-5;
@highlight-color: @red-5;
@warning-color: @gold-6;
@normal-color: #d9d9d9;
@white: #fff;
@black: #000;
```

基本变量：

```
@body-background: #fff;
@font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, 'Helvetica Neue', Arial,
  'Noto Sans', sans-serif, 'Apple Color Emoji', 'Segoe UI Emoji', 'Segoe UI Symbol',
  'Noto Color Emoji';
@font-variant-base: tabular-nums;
@font-feature-settings-base: 'tnum';
```

tabular-nums在font-variant-numeric中使用，用于控制数字、分数和序号标记的替代字形的使用。tabular-nums表示启用数字显示。使数字等宽，易于像表格那样对齐。等同于OpenType特性`tnum`。

tnum在`font-feature-settings`中使用，用于控制OpenType字体中的高级印刷功能。



用到的less函数

```
fade
tint
ceil
max
round
darken
shade
mix

~`colorPalette('@{success-color}', 3) `

@input-padding-vertical-base: max(
  round((@input-height-base - @font-size-base * @line-height-base) / 2 * 10) / 10 -
    @border-width-base,
  3px
);
@input-padding-vertical-sm: max(
  round((@input-height-sm - @font-size-base * @line-height-base) / 2 * 10) / 10 - @border-width-base,
  0
);
@input-padding-vertical-lg: ceil((@input-height-lg - @font-size-lg * @line-height-base) / 2 * 10) /
  10 - @border-width-base;

```


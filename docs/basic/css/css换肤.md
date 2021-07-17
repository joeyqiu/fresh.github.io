说起换肤功能，前端肯定不陌生，其实就是颜色值的更换，实现方式有很多，也各有优缺点

### 看需求是什么

一般来说换肤的需求分为两种：

1. 一种是几种可供选择的颜色/主题样式，进行选择切换，这种可供选择的主题切换不会很多
2. 另一种是需要自定义色值，或者通过取色板取色，可供选择的范围就很大了

### 如何实现

### 1. 对于可供选择的颜色/主题样式换肤的实现

- #### 一个全局class控制样式切换

切换的时候js控制样式的切换

- #### JS改变href属性值切换样式表，例如：

```
<link id="skincolor" href="skin-default.css" rel="stylesheet" type="text/css">
document.getElementById('#skincolor').href = 'skin-red.css';
```

这种方式需要维护几个主题样式表，js点击切换的时候通过改变css样式表链接来实现。 例如这个 [demo](https://www.zhangxinxu.com/study/200912/qq-home-page-skin-jquery.html)

这种实现对于，颜色和主题多了的时候，维护起来就很麻烦，需要同时维护 n 个样式文件，并且使用JS改变href属性会带来加载延迟，样式切换不流畅，体验也不好。

但如果是有包含不同复杂背景图片切换的时候，这种方式可以实现，但其他如下面要说的css变量 less modifyVars 就无法实现了

- #### HTML 的 rel 属性下的 alternate 实现： MDN [Alternative style sheets](https://developer.mozilla.org/en-US/docs/Web/CSS/Alternative_style_sheets)[#](https://www.cnblogs.com/leiting/p/11203383.html#3476249189)

示例：

```
<link href="reset.css" rel="stylesheet" type="text/css">
<link href="default.css" rel="stylesheet" type="text/css" title="Default Style">
<link href="fancy.css" rel="alternate stylesheet" type="text/css" title="Fancy">
<link href="basic.css" rel="alternate stylesheet" type="text/css" title="Basic">
```

所有样式表都可分为3类：

- 没有title属性，rel属性值仅仅是stylesheet的`<link>`无论如何都会加载并渲染，如reset.css；
- 有title属性，rel属性值仅仅是stylesheet的`<link>`作为默认样式CSS文件加载并渲染，如default.css；
- 有title属性，rel属性值同时包含alternate stylesheet的`<link>`作为备选样式CSS文件加载，默认不渲染，如red.css和green.css；

alternate意味备用，相当于是 css 预加载进来备用，所以不会有上面那种切换延时

但怎么用呢？禁用掉?

https://developer.mozilla.org/en-US/docs/Web/HTML/Element/link

link 的 disabled 属性

![disabled](https://img2018.cnblogs.com/blog/1207871/201909/1207871-20190906195557448-1626642938.png?ynotemdtimestamp=1626536153126)

使用JavaScript代码修改`<link>`元素**DOM对象的`disabled`值**为`false`，可以让默认不渲染的CSS开始渲染。实现 [demo](https://leitingting08.github.io/Front/example/alternative_style_sheets/index.html)

### 2. 对于制定动态色值换肤的实现

如果是要实现动态换肤，自定义色值，那上面的几种方式就不适合了。

先看下已有的实现有哪些方法

Element-UI 有换肤功能 [示例预览](https://elementui.github.io/theme-preview/#/zh-CN)[

](https://elementui.github.io/theme-preview/#/zh-CN)

实现原理： [官方解释](https://github.com/ElemeFE/element/issues/3054)

1. 先把默认主题文件中涉及到颜色的 CSS 值替换成关键词：https://github.com/ElementUI/theme-preview/blob/master/src/app.vue#L250-L274
2. 根据用户选择的主题色生成一系列对应的颜色值：https://github.com/ElementUI/theme-preview/blob/master/src/utils/formula.json
3. 把关键词再换回刚刚生成的相应的颜色值：https://github.com/ElementUI/theme-preview/blob/master/src/utils/color.js
4. 直接在页面上加 `style` 标签，把生成的样式填进去：https://github.com/ElementUI/theme-preview/blob/master/src/app.vue#L198-L211

看这个实现，还是比较麻烦的，想看看还有没有更优雅的方法来实现

Ant Design 的更换主题色功能是用 less 提供的 [modifyVars](http://lesscss.org/usage/#using-less-in-the-browser-modify-variables) 的方式进行覆盖变量来实现。

**less的 modifyVars**方法

**modifyVars方法是是基于 `less` 在浏览器中的编译来实现。\**所以在引入less文件的时候需要\**通过link方式引入**，然后基于less.js中的方法来进行修改变量

```
less.modifyVars({
  '@themeColor': 'blue'
});
```

link方式引入主题色文件

```
<link rel="stylesheet/less" type="text/css" href="./src/less/public.less" />
```

更改主题色事件

```
// color 传入颜色值
handleColorChange (color) {
    less.modifyVars({  // 调用 `less.modifyVars` 方法来改变变量值'
         @themeColor':color
         })
    .then(() => {
         console.log('修改成功');
    });
};
```

如果发现项目运行报错如下：

```
.bezierEasingMixin();
^
Inline JavaScript is not enabled. Is it set in your options?
```

那可能是没有开启 javascriptEnabled：true

在webpack配置里开启

```
{
      test: /\.less$/,
      loader: 'less-loader',
      options: {
             javascriptEnabled: true
       }
},
```

less方法仅限于用less的项目才能使用，查了下sass是没有类似 less.modifyVars 这种方法的。

那有没有通用一点的方法呢？于是就有了

**css 变量方法**

如果项目里用的不是less, 那么还是用css的方法，通用且容易操作，使用**css变量**来进行主题色的修改，替换主题色变量，然后用setProperty来进行动态修改

用法就是给变量加--前缀，涉及到主题色的都改成var(--themeColor)这种方式

用之前看下兼容性

![兼容性](https://img2018.cnblogs.com/blog/1207871/201908/1207871-20190802143842870-1700097756.png?ynotemdtimestamp=1626536153126)

[https://caniuse.com/#search=CSS%20Variables](https://caniuse.com/#search=CSS Variables)

大部分主流浏览器还是支持的，而且主要是操作起来够简便。

用法举例：

```
body{
   --themeColor:#000;
}
```

使用：

```
.main{
   color: var(--themeColor);
}
```

要修改主题色的话：

```
document.body.style.setProperty('--themeColor', '#ff0000');
```

### 总结

- 如果是固定的颜色切换，使用类名的方式会简单些，维护的复杂但是单纯不混杂
- 如果是动态让用户自己选的颜色，则使用变量的方式简单些。